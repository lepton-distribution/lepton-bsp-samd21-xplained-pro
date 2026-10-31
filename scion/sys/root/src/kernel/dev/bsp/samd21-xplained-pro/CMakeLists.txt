# BSP samd21-xplained-pro : paramètres hors manifeste (PA-7 Q3), inclus par le CMakeLists.txt racine
# du greffon noyau quand LEPTON_BOARD=samd21-xplained-pro. XML mklepton : LEPTON_MKCONF du preset.
# Sans réseau (décision 2026-10-05). Paquet constructeur : Microchip SAMD21_DFP 3.8.270 (famille
# samd21, LEPTON-PROVENANCE.md). DID 0x10010100 (SAMD21J18A rév. B), CPUID 0x410CC601 relus par
# la sonde.
# FreeRTOS (étape 7, module 7.3 ; décision 2026-10-06) : piles idle 128 mots et temporisateurs
# 256 mots (défauts 256 et 512) : avec 1 Ko et 2 Ko, le tas ne laisse que 484 o après le
# démarrage et lsh ne peut plus lancer de commande (pics relevés : idle 88 o, temporisateurs 236 o)
set(LEPTON_FREERTOS_CONFIG configMINIMAL_STACK_SIZE=128 configTIMER_TASK_STACK_DEPTH=256)
set(LEPTON_BOARD_UNAME_MACHINE cortexM0p-samd21)
# nom vu par uname : posé ici, sans entrée dans la table de kernelconf.h (ajout-coeur.md §4)
target_compile_definitions(lepton_options INTERFACE
  __KERNEL_CPU_DEVICE_NAME="${LEPTON_BOARD_UNAME_MACHINE}")
# CMSIS-Core 5 (core_cm0plus.h V5.0.9, exigé par le DFP de la famille samd21) avant le CMSIS 3.30
# de l'ISA (PA-25 Q4 : BEFORE, inexprimable dans le manifeste)
target_include_directories(lepton_options BEFORE INTERFACE
  ${LEPTON_SRC}/kernel/core/ucore/cmsis-5/CMSIS/Core/Include)

# carte réelle : console série de l'EDBG (sonde CMSIS-DAP : manifeste, champ openocd)
set(LEPTON_BOARD_SERIAL_PORT "" CACHE STRING
    "Port série de la console de la carte (ex. /dev/ttyACM0) : active le test de fumée sur carte")
