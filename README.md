# SafeTPA & SafeHomes Development Documentation

## Table of Contents
1. [Overview](#overview)
2. [Feature Implementation](#feature-implementation)
   - [Communication System](#communication-system)
   - [SafeTPA Rank-Based Teleportation System](#safetpa-rank-based-teleportation-system)
3. [Configuration & Permissions](#configuration--permissions)
4. [Development Timeline](#development-timeline)
5. [Expected Outcomes](#expected-outcomes)
6. [Next Steps](#next-steps)

---

## Overview
This document outlines the planned enhancements for the SafeTPA and SafeHomes plugins, focusing on improved player communication and rank-based teleportation settings. The primary goals are:
- **Inform players about teleportation cooldowns and home limits dynamically.**
- **Provide configurable teleportation delays based on player ranks.**
- **Implement instant teleportation for high-tier ranks.**

These changes aim to enhance the user experience and encourage rank upgrades by providing clear information on perks and limitations.

---

## Feature Implementation

### Communication System
#### Objective
To provide dynamic notifications informing players about cooldowns, limitations, and upgrade options.

#### Triggers
- **SafeTPA:** When a player reaches a teleport cooldown or is eligible for a reduced cooldown.
- **SafeHomes:** When a player is close to or has reached their home limit.

#### Notification Methods
1. **Chat Messages:**
   - Example:
   ```java
   player.sendMessage(ChatColor.RED + "You have reached your home limit!");
   ```

2. **Action Bar Notifications:**
   - Example:
   ```java
   player.spigot().sendMessage(ChatMessageType.ACTION_BAR, new TextComponent("You have 1 home slot left!"));
   ```

3. **Book GUI:**
   - Players receive an interactive book with rank and perk details.
   - Example:
   ```java
   ItemStack book = new ItemStack(Material.WRITTEN_BOOK);
   ```

4. **GUI Prompt (Optional):**
   - Clickable inventory UI to display ranks and benefits.

#### Configuration
- Allow server owners to enable/disable specific notification types in `config.yml`.
- PlaceholderAPI support for rank and limit placeholders.

---

### SafeTPA Rank-Based Teleportation System
#### Objective
To allow different teleportation execution times per rank and provide instant teleportation for specific cases.

#### New Features
1. **Configurable Teleport Delays per Rank**
   - Stored in `config.yml`:
   ```yaml
   teleport-delays:
     default: 5
     VIP: 3
     Legend: 0  # Instant teleport
   ```
   - Retrieved in code:
   ```java
   int delay = getConfig().getInt("teleport-delays." + playerRank, defaultDelay);
   ```

2. **Instant Teleportation for Certain Ranks**
   - Example logic:
   ```java
   if (playerHasRank(sender, "Legend") || playerHasRank(target, "Legend")) {
       executeTeleport(sender, target, 0);
   }
   ```

3. **Permissions-Based Execution**
   - Use permissions like `safetpa.instant` for instant teleportation:
   ```java
   if (player.hasPermission("safetpa.instant")) {
       executeTeleport(player, target, 0);
   }
   ```

---

## Configuration & Permissions

### Configuration (`config.yml`)
```yaml
teleport-delays:
  default: 5
  VIP: 3
  Legend: 0
notifications:
  chat: true
  action-bar: true
  book: false
  gui: true
```

### Permissions
- `safetpa.instant` → Grants instant teleport.
- `safetpa.delay.bypass` → Bypasses teleport cooldowns.
- `safehomes.limit.increase` → Allows setting more homes beyond default limit.

---

## Development Timeline

| Phase | Task | Estimated Time |
|-------|------|---------------|
| **Phase 1** | Implement communication system (chat, action bar, book) | 3-5 days |
| **Phase 2** | SafeTPA enhancements (configurable delays, instant teleport) | 5-7 days |
| **Phase 3** | Testing, optimizations, and documentation | 3-4 days |

**Estimated Total Time: ~2 Weeks (10-14 days)**

---

## Expected Outcomes
- Players receive clear, dynamic messages about teleport/home limits and upgrade options.
- Different ranks have varying teleport delays based on configurations.
- High-tier ranks enjoy instant teleportation perks.
- Server owners have full control over settings via `config.yml`.

---

## Next Steps
1. Gather feedback from stakeholders on notification and teleportation methods.
2. Begin implementation and iterative testing.
3. Finalize features and optimize performance before release.
