# dwmstatus

Daemon that periodically updates the dwm status bar.

## Usage

1. Clone the repository to `~/.local/src` and build the project.
    ```sh
    git clone https://github.com/joeyshi12/dwmstatus ~/.local/src/dwmstatus
    cd ~/.local/src/dwmstatus
    make
    ```
2. Add `~/.local/src/dwmstatus/dwmstatus &` as an [autostart](https://wiki.archlinux.org/title/Autostarting) command.

## Customization

Each module is a string buffer that is copied onto `status` in the main function of `dwmstatus.c`.
To add a battery module, find out which folder contains battery information:
find a folder named `/sys/class/power_supply/BAT0` or `/sys/class/power_supply/BAT1`.
Then, use the following helper function to format the string for the battery module:

```c
void get_battery(char *buf, size_t size) {
    FILE *file = fopen("/sys/class/power_supply/<BAT0 or BAT1>/capacity", "r");
    if (!file) {
        snprintf(buf, size, "BAT ?");
        return;
    }
    unsigned int capacity;
    fscanf(file, "%u", &capacity);
    fclose(file);
    snprintf(buf, size, "BAT %u", capacity);
}
```
