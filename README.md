
KiCad utilities
===============

## schlib directory

**checklib.py**: Script for checking [KLC][] compliance of schematic symbol libraries.

**schlib.py**: A Python module for parsing KiCad's schematic library file format.

**test_schlib.sh**: A shell script used to validate the generation of files of the schlib module.

**fix-pins.py**: A script created to help adapt existing library files to the [KiCad Library Convention][KLC] by testing some cases of x/y "wrong" pin positions and trying to fix them. The description of the cases are explained in the header of the script file.

**move_part.py**: Script to move components between libraries.

**autogen/stm32/**: Automatic STM32 library generation from pin files provided by ST. Detailed information can be found in [autogen/stm32/README.md](schlib/autogen/stm32/README.md)

## sch directory

**sch.py**: A Python module for parsing KiCad's schematic file format.

**test_sch.sh**: A shell script used to validate the generation of files of the sch module.

**add_part_number.py**: This script is used to add/edit part number fields in the schematic files.

**update_footprints.py**: This script updates the footprint fields of `.sch` files using a `.csv` file as input.

## pcb directory

**check_kicad_mod.py**: Script for checking [KLC][] compliance of footprint files.

**kicad_mod.py**: A Python module for loading, editing, and saving KiCad footprint files.

**check_3d_coverage.py**: Script for checking which KiCad footprints in a `.pretty` library have 3D models. It also shows unused 3D model files.

[KLC]: http://kicad-pcb.org/libraries/klc/

How to use
==========

## Schematic Library Checker

    # first get into schlib directory
    cd schlib

    # run the script passing the files to be checked
    ./checklib.py path_to_lib1 path_to_lib2

    # to check a specific component you can use the -c flag
    ./checklib.py -c component_name path_to_lib1

    # run the following command to see other options
    ./checklib.py -h

## Adding Part Number (PN) to Schematic Files

    # first get into sch directory
    cd sch

    # use the following command to add empty "MPN" fields in the schematic files
    ./add_part_number.py path_to_sch1 path_to_sch2

    # use the following command to add/edit the PN field using the passed csv file
    # the default behaviour is search for "Reference(s)" and "MPN" columns in the csv
    # the BOM generated by bom_csv_grouped_by_value plugin can be used after
    # manually add the MPN field in the Collated Components section (for example)
    ./add_part_number.py --bom-csv=path_to_bom_csv path_to_sch_files/*.sch

    # run the following command to see other options
    ./add_part_number.py -h


## Footprint Checker

    # first get into pcb directory
    cd pcb

    # run the script passing the files to be checked
    ./check_kicad_mod.py path_to_fp1 path_to_fp2

    # run the following command to see other options
    ./check_kicad_mod.py -h


## 3D Coverage Checker

    # first get into pcb directory
    cd pcb

    # run the script to check all footprints
    ./check_3d_coverage.py

    # run the script to check only the specified .pretty folder
    ./check_3d_coverage.py --prettty Housings_SOIC

    # run the following command to see other options
    ./check_3d_coverage.py -h


Notice
======

The scripts use a different algorithm to generate files in relation to the KiCad saving action. That will result output files with more modified lines than expected, because the line generally are repositioned. However, the file still functional.

Always check the generated files by opening them on KiCad. Additionally, if you are working over a git repository (if not, you should) you can commit your work before proceed with the scripts, this will put you safe of any trouble. Also, you would use git diff to give a look at the modifications.
