---
layout: page
title: News
permalink: /news/
---

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
- :calendar: **Jun 8, 2024**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
version 1.1.8
    - New features
        - Some SMTP server send their list of supported extensions in multiple
        buffers like Zoho Mail. The EHLO command when in uncrypted mode, now
        supports receiving multiple buffers. In return, a delay of one second
        must be added for each segment sent by the SMTP server. For SMTP
        servers that send the list of supported extensions in a single segment
        like Gmail and Live, no additional delay is added for the EHLO command.
        This doesn't affect the other commands.
        - Now when we send an email to multiple recipients (to or cc), the
        recipients appears as a single mail header instead of multiple headers.
        The old method was not RFC 5322 compliant.
- :calendar: **Jan 3, 2024**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
version 1.1.7
    - New features
        - Added support for the XOAUTH2 authentication method.
        - Added a new flag in the different SMTP client classes to indicate whether we
want to send emails in batch (getBatchMode/setBatchMode). In this mode the connection to an
SMTP server will only be made once when the first email is sent and will
remain open until the client instance is destroy.
        - Added the authentication feature on the SMTPClient class.
        - Added a new flag on the ForcedSecureSMTPClient and OpportunisticSecureSMTPClient
to indicate whether we accept self signed certificate
(getAcceptSelfSignedCert/setAcceptSelfSignedCert).
- :calendar: **Nov 10, 2023**: Released [The Warrior](https://github.com/jeremydumais/TheWarrior)
version 0.3.2.
    - Map Editor
        - New features
            - Updated the upper toolbar to make the selected action visually
            marked as selected.
            - It is now possible to move the differents toolbar to the left and
            the right of the screen. You can also close them and made them
            floatable. The menu View > Toolbar has been added to toggle
            toolbars visibility.
            - Added a debugging info toolbar that is hidden by default. You can
            open it through the View > Toolbar menu.
            - The map section of the editor is supporting dynamic size.
            - Added the zoom feature to the map.
            - Removed the NPC component that was unused for now.
            - Relocated MainForm components as real QWidget independant
            components.
            - Implemented the Double click and Del key press in the Texture List
            Component.
            - Implemented the Double click and Del key press in the Tile
            Properties Component for the Event list.
            - Enhanced the current texture edit form by using the common ui
            texture edit form controller.
        - Bug fixes
            - Solved Issue #10: When you unselect a tile the tiles details
            stays in the Tile Properties Component.
- :calendar: **Oct 11, 2023**: Released [The Warrior](https://github.com/jeremydumais/TheWarrior)
version 0.3.1.
    - Map Editor
        - New features
            - Added the assign/clear monster zones to the map feature.
        - Bug fixes
            - When selecting multiple tiles (e.g. with a texture assigned), the
            resulting selection could be incorrect if you stop on the edge of a
            tile.
    - Monster Editor
        - New features
            - Implemented the CLI program options to be able to load a monster
            store directly from the CLI.
            - Draw the tiles zones in the Add & Edit Texture form.
    - Item Editor
        - New features
            - Implemented the CLI program options to be able to load a monster
            store directly from the CLI.
            - Draw the tiles zones in the Add & Edit Texture form.
- :calendar: **Sep 23, 2023**: Released [The Warrior](https://github.com/jeremydumais/TheWarrior)
version 0.3.0.
    - Game
        - New features
            - Add gold stats in the Character window
    - Map Editor
        - New features
            - Added a setting form to manage item stores.
            - Added a setting form to manage monster stores.
            - Added the Monster zones component in the main form.
    - Monster Editor
        - Create the Monster Editor to manage monsters with their stats.
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
    - Fix a bug where the text color is not reset for next line.
- :calendar: **Mar 1, 2022**: Released [BeaverTodos](https://github.com/jeremydumais/BeaverTodos)
version 0.2.0.
    - Added the Remove a todo feature.
    - Added the Edit a todo feature.
    - Added the Purge the todo list feature.
    - You can now get the next todo to work on
    - Added the Fetch the details of a todo feature.
    - Added the usage when no command is supplied
- :calendar: **Feb 6, 2022**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library) version 1.1.4.
    - Added the BUILD_TESTING flag in the CMake project so the unit tests are not build by default
    and Google Test is no longer required.
    - Added a new uninstall target in the CMake project.
    - Added a new ErrorResolver class to get a string representation (error message) of a return
    code obtained by the sendMail method from the different classes of SMTP clients.
    - Two new methods has been added to the SMTPClientBase class : getErrorMessage and
    getErrorMessage_r. Those two methods can be used to get a string representation (error message)
    of a return code obtained by the sendMail method from the different classes of SMTP clients.
    - The Google Test dependency branch has been switched from master to main.
    - Code: The using namespace std; has been removed as it is considered bad practice.
    - The Windows documentation has been updated to explain how to use OPENSSL_ROOT_DIR variable.
    - Added documentation in all headers files for public methods.
    - The exception classes AttachmentError and CommunicationError has been removed.
- :calendar: **Aug 1, 2021**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library) version 1.1.3.
    - Renamed the class SSLSmtpClient to OpportunisticSecureSMTPClient but added a typedef and kept
    the sslsmtpclient.h for backward compatibility.
    - Added support for forced ssl connection (SMTP port 465) via the ForcedSecureSMTPClient class
    - Added the new base class SecureSMTPClientBase to centralize the common code of the classes
    ForcedSecureSMTPClient and OpportunisticSecureSMTPClient.
- :calendar: **May 27, 2021**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library) version 1.1.2.
    - Refactored the code of the SmtpClient class to inherit the SmtpClientBase class.
    - You must now call the method getCommunicationLog() instead of getServerReply().
- :calendar: **May 6, 2021**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library) version 1.1.1.
    - Added support for the cc and bcc field in the sendMail method
- :calendar: **Mar 28, 2021**: Released [TeacherHelper](https://github.com/jeremydumais/TeacherHelper) version 1.1.0.
    - The referential integrity constraint in the different tables has been enforced.
    - Added a version table and class to be able to implement the in-software database upgrade.
    - Added the first report Class Assessments Summary Report.
    - Class form : Ensure the member list resize with the window.
    - Assessment Form: Let the user choose the max result of the assessment. Not necessarily 100.
    - Fixed compilation warnings in Windows ans OS X.
- :calendar: **Jan 16, 2021**: Released [The Warrior](https://github.com/jeremydumais/TheWarrior)
version 0.1.0.
    - Game
        - First steps in the game application.
        - Displayed the map using modern OpenGL.
        - Displayed the player on the map.
        - Added the window resize feature.
        - Added an option to display the FPS.
    - Map Editor
        - Added the map resize feature
        - Added a tool to configure the cannot step-on tile.

