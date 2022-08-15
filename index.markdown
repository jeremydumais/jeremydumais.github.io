---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home

---

Welcome to Jed# Software. Our mission? Provide free and open source software.


## Latest news

- **Jul 15, 2022**: Released The Warrior version 0.2.0. 
    - Game
        - New features
            - Added GLTextBox support to display messages in the game.
            - Added a message pipeline in the game.
            - Added GLPopupWindow support to display inventory, character infos etc.
            - Added the Main menu when in game map mode.
            - Added the inventory window mode when in game map mode. You can equip, move and drop items.
            - Added the character window mode when in game map mode.
            - Added a mechanism to make sure we cannot open the chest multiple times.
            - Added joystick support for the different windows.
        - Bug fixes
            - Fix joystick player movement issue and start working on the InputDevicesState class.
        - Item Editor
            - Create the Item Editor to manage items, stats items, weapons and armors.
        - Map Editor
            - New features
                - Added the X and Y coordinates of the selected tile.
                - Added a flag to indicate if an object texture needs to appear in front or behind the player.
                - Added tile's trigger and action feature.
                - Added icons in the toolbar.
                - Added the Edit, View and Clear Blocked Border Mode features.
            - Bug fixes
                - Fix an issue where the initShader method of the GLTextService and GLTileService was not returning any value on success.
- **Mar 9, 2022**: Released BeaverTodos version 0.2.2.
    - Fix an issuse where you couldn't close a todo if a completed todo had the same id.
- **Mar 2, 2022**: Released BeaverTodos version 0.2.1.
    - Fix a bug where the text color is not reset for next line.


## Latest Blog posts

