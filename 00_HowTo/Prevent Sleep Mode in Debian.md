#type/HowTo #topic/Debian/Sleep-Mode-Prevent #for/all 

To prevent sleep mode in Debian while using the dwm window manager, you can modify the power management settings. You can disable sleep mode by using the command `xset s off` to turn off screen saver and `xset -dpms` to disable DPMS (Energy Star) features.
## Disabling Sleep Mode in Debian with dwm

To prevent sleep mode while using the dwm window manager in Debian, you can adjust the power management settings through specific commands.

### Steps to Disable Sleep Mode

1. Turn Off Screen Saver
```bash
xset s off
```  
2. Disable DPMS Features
```bash
xset -dpms
```    

### Summary of Commands

| Command      | Purpose                              |
| ------------ | ------------------------------------ |
| `xset s off` | Disables the screen saver            |
| `xset -dpms` | Disables DPMS (Energy Star) features |

By following these steps, you can effectively prevent your Debian system from entering sleep mode while using the dwm window manager.