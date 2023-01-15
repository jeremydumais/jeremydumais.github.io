---
layout: page
title: News :newspaper:
permalink: /news/
---

- **Jan 15, 2023**: Completed the Single Linked List in the [Study the RoadMap 
of Computer Science](https://github.com/jeremydumais/Study_RoadMap_ComputerScience)
(Challenge 2023). See my github repo for more details.
- **Dec 31, 2022**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library)
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
- **Jul 15, 2022**: Released [The Warrior](https://github.com/jeremydumais/TheWarrior)
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
- **Mar 9, 2022**: Released [BeaverTodos](https://github.com/jeremydumais/BeaverTodos)
version 0.2.2.
    - Fix an issuse where you couldn't close a todo if a completed todo had the same id.
- **Mar 2, 2022**: Released [BeaverTodos](https://github.com/jeremydumais/BeaverTodos)
version 0.2.1.
    - Fix a bug where the text color is not reset for next line.
- **Mar 1, 2022**: Released [BeaverTodos](https://github.com/jeremydumais/BeaverTodos)
version 0.2.0.
    - Added the Remove a todo feature.
    - Added the Edit a todo feature.
    - Added the Purge the todo list feature.
    - You can now get the next todo to work on
    - Added the Fetch the details of a todo feature.
    - Added the usage when no command is supplied
- **Feb 6, 2022**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library) version 1.1.4.
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
- **Aug 1, 2021**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library) version 1.1.3.
    - Renamed the class SSLSmtpClient to OpportunisticSecureSMTPClient but added a typedef and kept 
    the sslsmtpclient.h for backward compatibility.
    - Added support for forced ssl connection (SMTP port 465) via the ForcedSecureSMTPClient class
    - Added the new base class SecureSMTPClientBase to centralize the common code of the classes 
    ForcedSecureSMTPClient and OpportunisticSecureSMTPClient.
- **May 27, 2021**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library) version 1.1.2.
    - Refactored the code of the SmtpClient class to inherit the SmtpClientBase class.
    - You must now call the method getCommunicationLog() instead of getServerReply().
- **May 6, 2021**: Released [C++ SMTP Client Library](https://github.com/jeremydumais/CPP-SMTPClient-library) version 1.1.1.
    - Added support for the cc and bcc field in the sendMail method
- **Mar 28, 2021**: Released [TeacherHelper](https://github.com/jeremydumais/TeacherHelper) version 1.1.0.
    - The referential integrity constraint in the different tables has been enforced.
    - Added a version table and class to be able to implement the in-software database upgrade.
    - Added the first report Class Assessments Summary Report.
    - Class form : Ensure the member list resize with the window.
    - Assessment Form: Let the user choose the max result of the assessment. Not necessarily 100.
    - Fixed compilation warnings in Windows ans OS X.
- **Jan 16, 2021**: Released [The Warrior](https://github.com/jeremydumais/TheWarrior)
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

