


                                                    +-----+                                             +-----+                                         +-----+                                 +-------+
                                                    | AS  |                                             | SR  |                                         | DR  |                                 | SySR  |
                                                    +-----+                                             +-----+                                         +-----+                                 +-------+
                                     ----------------\ |                                                   | --------------\                               | --------------\                        | --------------\
                                     | Prerequisite: |-|                                                   |-| operations: |                               |-| operations: |                        |-| operations: |
                                     |---------------| |                                                   | |-------------|                               | |-------------|                        | |-------------|
                                                       |                                                   |                                               |                                        | 
                                                       |                                                   |                                               |                                        |
                                                       |                                                   |                                               |                                        |
                      -------------------------------\ |                                                   |                                               |                                        |
                      | Has valid system certificate |-|                                                   |                                               |                                        |
                      |------------------------------| |                                                   |                                               |                                        |
                             ------------------------\ |                                                   |                                               |                                        |
                             | Has SR address & port |-|                                                   |                                               |                                        |
                             |-----------------------| |                                                   |                                               |                                        |
                 ------------------------------------\ |                                                   |                                               |                                        |
                 | Has SR uri and request schema for |-|                                                   |                                               |                                        |
                 | Query DR's register device        | |                                                   |                                               |                                        |
                 |-----------------------------------| |                                                   |                                               |                                        |
                                                       | where is register-device service ?                |                                               |                                        |
                                                       |-------------------------------------------------->|                                               |                                        |
                                                       |                                                   | ---------------------------------------\      |                                        |
                                                       |                                                   |-| get register-device                  |      |                                        |
                                                       |                                                   | | DR system name and /port/uri from DB |      |                                        |
                                                       |                                                   | |--------------------------------------|      |                                        |
                                                       |                                                   |                                               |                                        |
                                                       |             DR's register-device address/port/uri |                                               |                                        |
                                                       |<--------------------------------------------------|                                               |                                        |
                -------------------------------------\ |                                                   |                                               |                                        |
                | Has register-device request schema |-|                                                   |                                               |                                        |
                |------------------------------------| |                                                   |                                               |                                        |
                                   ------------------\ |                                                   |                                               |                                        |
                                   | Has device name |-|                                                   |                                               |                                        |
                                   |-----------------| |                                                   |                                               |                                        |
                                   ------------------\ |                                                   |                                               |                                        |
                                   | Has MAC address |-|                                                   |                                               |                                        |
                                   |-----------------| |                                                   |                                               |                                        |
                                                       |                                                   |                                               |                                        |
                                                       | register device                                   |                                               |                                        |
                                                       |-------------------------------------------------------------------------------------------------->|                                        |
                                                       |                                                   |                                               | -----------------------------------\   |
                                                       |                                                   |                                               |-| If device is not exist yet, then |   |
                                                       |                                                   |                                               | | create else update               |   |
                                                       |                                                   |                                               | |----------------------------------|   |
                                                       |                                                   |            return HTTP status code 200 or 201 |                                        |
                                                       |<--------------------------------------------------------------------------------------------------|                                        |
                 ------------------------------------\ |                                                   |                                               |                                        |
                 | Has SR uri and request schema for |-|                                                   |                                               |                                        |
                 | Query SySR's register system      | |                                                   |                                               |                                        |
                 |-----------------------------------| |                                                   |                                               |                                        |
                                                       | where is register-system service?                 |                                               |                                        |
                                                       |-------------------------------------------------->|                                               |                                        |
                                                       |                                                   | -----------------------------\                |                                        |
                                                       |                                                   |-| get SySR's register-system |                |                                        |
                                                       |                                                   | | /port/uri from DB          |                |                                        |
                                                       |                                                   | |----------------------------|                |                                        |
                                                       |                                                   |                                               |                                        |
                                                       |    return SySR's register-system address/port/uri |                                               |                                        |
                                                       |<--------------------------------------------------|                                               |                                        |
                -------------------------------------\ |                                                   |                                               |                                        |
                | Has register-system request schema |-|                                                   |                                               |                                        |
                |------------------------------------| |                                                   |                                               |                                        |
                    ---------------------------------\ |                                                   |                                               |                                        |
                    | Still has device name for      |-|                                                   |                                               |                                        |
                    | register-system request schema | |                                                   |                                               |                                        |
                    ---------------------------------\ |                                                   |                                               |                                        |
                    | Has system info for            |-|                                                   |                                               |                                        |
                    | register-system request schema | |                                                   |                                               |                                        |
                    |--------------------------------| |                                                   |                                               |                                        |
                                                       | register system                                   |                                               |                                        |
                                                       |------------------------------------------------------------------------------------------------------------------------------------------->|
                                                       |                                                   |                                               |                                        | ------------------------------------\
                                                       |                                                   |                                               |                                        |-| check if the caller`s certificate |
                                                       |                                                   |                                               |                                        | | matches the to-be-created system  |
                                                       |                                                   |                                               |                                        | |-----------------------------------|
                                                       |                                                   |                                               |                           check-device |
                                                       |                                                   |                                               |<---------------------------------------|
                                                       |                                                   |                                               | ------------------------------\        |
                                                       |                                                   |                                               |-| check if device exist in db |        |
                                                       |                                                   |                                               | |-----------------------------|        |
                                                       |                                                   |                                               |                                        |
                                                       |                                                   |                                               | return check-device result             |
                                                       |                                                   |                                               |--------------------------------------->|
                                                       |                                                   |                                               |                                        | -----------------------------------\
                                                       |                                                   |                                               |                                        |-| if system is not exist yet, then |
                                                       |                                                   |                                               |                                        | | create else update               |
                                                       |                                                   |                                               |                                        | |----------------------------------|
                                                       |                                                   |                                               |     return HTTP status code 200 or 201 |
                                                       |<-------------------------------------------------------------------------------------------------------------------------------------------|
-----------------------------------------------------\ |                                                   |                                               |                                        |
| Has SR uri and request schema for register service |-|                                                   |                                               |                                        |
|----------------------------------------------------| |                                                   |                                               |                                        |
                                                       |                                                   |                                               |                                        |
                                                       | register-service                                  |                                               |                                        |
                                                       |-------------------------------------------------->|                                               |                                        |
                                                       |                                                   | -------------------------------------------\  |                                        |
                                                       |                                                   |-| check if the caller`s certificate        |  |                                        |
                                                       |                                                   | | matches the system in the request schema |  |                                        |
                                                       |                                                   | ------------------------------------\------|  |                                        |
                                                       |                                                   |-| if service is not exist yet, then |         |                                        |
                                                       |                                                   | | create else update                |         |                                        |
                                                       |                                                   | |-----------------------------------|         |                                        |
                                                       |                return HTTP status code 200 or 201 |                                               |                                        |
                                                       |<--------------------------------------------------|                                               |                                        |
                                                       |                                                   |                                               |                                        |

