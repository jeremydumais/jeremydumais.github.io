---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home

---

Welcome to Jed# Software. My mission? Provide free and open source software.


## Latest news :newspaper:

- :calendar: **Jul 23, 2025**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
version 1.1.11
    - New features
        - Adjust the line length of the MIME attachments to 76 (excluding CRLF)
        to comply with RFC 2045.
        - Add a log level to the multiple clients. The log level is for the
        level of details of the communication log between the client and the
        server. Choices are : None, ExcludeAttachmentsBytes and Full. Default
        is ExcludeAttachmentsBytes.
        - Add the Date header field in outgoing emails to comply with RFC 2822.
        This is a required field and it was missing in the previous versions.
    - Bug fixes
        - Fix the error 554 5.0.0 ("failed to create parser: unexpected EOF")
        when sending multipart messages via ProtonMail Bridge due to missing
        closing MIME boundary (--sep--).
        - Prevented catastrophic backtracking in isEmailAddressValid() regex
        that caused crashes when validating complex email addresses (e.g.,
        from mailersend.com). Updated regex to avoid unescaped dots and added a
        more robust pattern.
- :calendar: **Jul 6, 2025**: Released [The Warrior](https://github.com/jeremydumais/TheWarrior)
version 0.3.3.
    - Map Editor
        - Changes
            - Move the editor configuration file into the folder "Jed#
            Software/The Warrior - Map Editor" instead of "The Warrior - Map
            Editor"
            - Only the tiles that are visible on the screen are now displayes.
            This result in lower CPU usage.
        - New features
            - Persist the position status of toolbars.
            - Persist the visibility status of toolbars.
            - Persist the visiblity status of the display grid menu item.
            - Add a shortcut to move the map -> Alt + Click.
            - Enable the zoom feature with the mouse wheel.
            - Implement the undo/redo actions.
            - Add a Picker tool to get a texture an object from a tile.
            - Add support to change multi-selection tile properties.
            - Add bottom tabs to change the map view (ex: CanStep, MonsterZone,
            BlockedBorder etc).
            - Persist the "Use only one monster zone for all the map" field in
            the GameMap class.
            - Add a MonsterZone selection component in the Tile Property panel.
            - Disable the MonsterZone combobox in tile props component when "Use
            only one monster zone for all the map" is selected.
            - Implement the copy/paste of tiles feature.
        - Bug fixes
            - Solved Issue #6: Application crash when you multiselect a zone
            that ends outside of the GLComponent.
            - Solved Issue #14: When creating the first monster zone, it is
            assigned automatically to the selected tiles.
            - Solved Issue #15: Undo history is not update when we change tile
            value via the Tile Property panel.
            - Solved Issue #13: Application crash when you apply texture on map
            then same on another file.
            - Solved Issue #9: Unable to select a tile when the map is moved in
            the lower right corner.
    - Item Editor
        - Changes
            - Move the editor configuration file into the folder "Jed#
            Software/The Warrior - Item Editor" instead of "The Warrior - Item
            Editor"
    - Monster Editor
        - Changes
            - Move the editor configuration file into the folder "Jed#
            Software/The Warrior - Monster Editor" instead of "The Warrior -
            Monster Editor"
- :calendar: **May 23, 2025**: [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
is now on [Conan](https://conan.io/center/recipes/cpp-smtpclient-library)! :partying_face:
- :calendar: **Apr 19, 2025**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
version 1.1.10
    - New features
        - Add support for macOS.
    - Bug fixes
        - Fix the install/uninstall process of the library.
        - Solve the issue #38 where STARTTLS is not recognized if it is returned
        as the last response from the mail server.
- :calendar: **Mar 30, 2025**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
version 1.1.9
    - New features
        - Rework the build system to support static build and to generate
        correct release version.
        - The build configuration now works with multi-config generators like
        Visual Studio.
        - The default build configurations in Visual Studio has been changed to :
            - x64-Debug
            - x64-Debug-Static
            - x64-Debug-WithUnitTests
            - x64-Release
            - x64-Release-Static
            - x64-Release-WithUnitTests

<hr style="border: 1px solid #333333"/>

## Latest Blog posts :green_book:

