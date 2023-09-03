---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home

---

Welcome to Jed# Software. Our mission? Provide free and open source software.


## Latest news :newspaper:

- :calendar: **Sep 3, 2023**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
version 1.1.6
    - New features
        - Added support in the attachment class for Content-ID. It will be
        really useful to uniquely identify and reference resources to embed in
        the message.
- :calendar: **Jan 22, 2023**: Completed the Double Linked List in the [Study the RoadMap
of Computer Science](https://github.com/jeremydumais/Study_RoadMap_ComputerScience)
(Challenge 2023). See my github repo for more details.
- :calendar: **Jan 15, 2023**: Completed the Single Linked List in the [Study the RoadMap
of Computer Science](https://github.com/jeremydumais/Study_RoadMap_ComputerScience)
(Challenge 2023). See my github repo for more details.
- :calendar: **Dec 31, 2022**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
version 1.1.5
    - New features
        - Added OpenSSL variables in CMakeLists to be able to specify include,
        library path and library files.
        - A set of new classes has been added to the jed_utils::cpp namespace
        to provide a pure C++ way to consume the library. This is the new
        standard from version 1.1.5. See the new class documentation in the wiki.
    - Updated
        - Changed the ErrorResolver field mErrorMessage from an std::string to an
        char * to keep a Plain C Interface.
        - Code formatting applied throughout the project using cpplint and following
        Google's C++ style guide.
        - Configured Linux Socket connect function to non-blocking mode to make
        the SmtpClientBase timeout working as expected.
        - Added WSACleanup error return code to the communication log.
    - Bug fixes
        - Changed the size of the communication log buffer from static (4096 bytes) to an
        auto-growing dynamic buffer.
    - Security fixes
        - Replaced all insecure strcpy by functions that support length arguments like
        strncpy.
- :calendar: **Jul 15, 2022**: Released [The Warrior](https://github.com/jeremydumais/TheWarrior)
version 0.2.0.
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
- :calendar: **Mar 9, 2022**: Released [BeaverTodos](https://github.com/jeremydumais/BeaverTodos)
version 0.2.2.
    - Fix an issuse where you couldn't close a todo if a completed todo had the same id.
- :calendar: **Mar 2, 2022**: Released [BeaverTodos](https://github.com/jeremydumais/BeaverTodos)
version 0.2.1.


## Latest Blog posts :green_book:

