---
title: "Customization"
permalink: /customization/
layout: page
nav_order: 3
---
# Special Keys on the default keymaps

0. `FN2 + 8`: Disable LEDs
0. `FN2 + 9`: Enable LEDs / Switch profiles
0. `FN2 + 0`: Change brightness
0. `FN2 + -`: Change animation speed

# Anne Pro 2 Specific Keycodes

These are made to control annepro2-shine. You can use them with any key. Check the default profiles. They're used just
like any other keycode.

0. **KC_AP2_BT1:**               Switch to bluetooth 1
0. **KC_AP2_BT2:**               Switch to bluetooth 2
0. **KC_AP2_BT3:**               Switch to bluetooth 3
0. **KC_AP2_BT4:**               Switch to bluetooth 4
0. **KC_AP2_BT_UNPAIR:**         Unpair bluetooth
0. **KC_AP2_USB:**               Switch to device using the USB
0. **KC_AP_LED_ON:**             Enable LEDs
0. **KC_AP_LED_OFF:**            Disable LEDs
0. **KC_AP_LED_NEXT_PROFILE:**   Go to next profile
0. **KC_AP_LED_PREV_PROFILE:**   Go to previous profile
0. **KC_AP_LED_NEXT_INTENSITY:** Go to next brightness intensity
0. **KC_AP_LED_SPEED:**          Go to next speed

# Keymaps

Unfortunately, the Anne Pro 2 currently does not have support for automatic QMK configurators. This means you have to
manually edit the file itself. These profiles are located in `annepro_qmk/keyboards/annepro2/keymaps/` as separate
folders for each profile. Each profile has a `keymap.c` file. You can copy one of the default profile folders as a starting
template and edit it to your preference. Recompiling the firmware will automatically compile your custom keymap to
whatever name you named the folder.

# Layout Customization

The default keymaps already have layouts based on the stock Anne Pro 2 layout. Changing layouts is easy.
[Check here for keycodes](https://beta.docs.qmk.fm/tutorial/newbs_building_firmware#customize-the-layout-to-your-liking).
Each layer keymap, such as [_BASE_LAYER], is shaped to look similar to the keyboard. This will make it easier to 
customize. You can also see the ASCII graphic of the keyboard above it to see what the layer represents. You can also
do much more than this. Just check the QMK docs for more info.

As an example, to make the ESC key produce an "A," switch `KC_ESC` to `KC_A`.

# Layers

Layers are defined as different keymaps. Check [this section](https://beta.docs.qmk.fm/using-qmk/software-features/feature_layers)
of the QMK docs for more info on them. This is also how you can create your own custom MAGIC FN key and tap layers.

# Tap layers

As what the previous link talks about, LT() is the function to create TAP buttons. For more information on them,
check out [this section](https://beta.docs.qmk.fm/using-qmk/software-features/tap_hold#tapping-force-hold) of the docs.

The default keymap also includes a feature where tapping then holding a tap key will repeat the tap keycode.

Additionally, the MAGIC FN key is FN1 by default. You can change it to FN2 or any other layer as needed. In fact, you
can change the MAGIC FN key to be any key, not just caps lock.

# KeymapCEditor

There is an extension for VSCode called [KeymapCEditor](https://marketplace.visualstudio.com/items?itemName=Ciantic.keymapceditor-vsc)
that gives a user interface, making it easier to create mappings. After installing it, open the extension by pressing `CTRL+SHIFT+P` and
search `keymap`. You should see `Show KeymapCEditor ...`. Select it to open it. The Anne Pro 2 isn't officially supported
by QMK, so you have to choose a keyboard with the same layout. Choose "Practice 60", and selecting "ANSI" for type.
When changing a key's mapping, you still need to manually enter the QMK keycode. The extension currently doesn't support
creating macros, so you still need to do that by hand.

# Macros

To create macros, follow [this guide](https://beta.docs.qmk.fm/using-qmk/advanced-keycodes/feature_macros). You may need
to be familiar with C. You can also follow [this youtube video](https://www.youtube.com/watch?v=WITZaRsoO_Q) for a
visual guide instead.

# Changing Default LED Profile

You should see this function in your `keymap.c` file if it's based on one of the defaults:

```cpp
// Code to run after initializing the keyboard
void keyboard_post_init_user(void) {
    // Here are two common functions that you can use. For more LED functions, refer to the file "qmk_ap2_led.h"

    // annepro2-shine disables LEDs by default. Uncomment this function to enable them at startup.
    // annepro2LedEnable();

    // Additionally, it also chooses the first LED profile by default. Refer to the "profiles" array in main.c in
    // annepro2-shine to see the order. Replace "i" with the index of your preferred profile. (i.e the RED profile is index 0)
    // annepro2LedSetProfile(i);
}
```

This function will run after the keyboard has successfully started. Uncomment annepro2LedEnable() by removing the two
slashes before it to enable LEDs at startup. By default, it is disabled. Uncomment annepro2LedSetProfile(i) to change
the default color profile. Replace i with one of the numbers in the list below that corresponds to a profile:

0: Full red

1: Full green

2: Full blue

3: Horizontal rainbow

4: Vertical Rainbow

5: Low-FPS Animated Vertical Rainbow

6: Animated Vertical Rainbow

7: Animated Horizontal Rainbow

8: Animated Breathing

9: Animated Red Vertical Wave

10: Animated Spectrum

11: Reactive Fade

This corresponds to the same order when switching color profiles manually.