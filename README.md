# Klipper Configuration for Custom Ender 3-like Printer

## Overview
This configuration is designed for a custom Ender 3-like 3D printer with the following specifications:
- **Mainboard**: BigTreeTech Octopus EX Max
- **Probe**: Beacon3D Eddy Current Probe (Slim Connector)
- **Z-axis**: Dual Z motors for improved stability
- **Kinematics**: Cartesian

## Configuration Files

### Main Configuration Files
- `printer.cfg` - Main configuration file (includes all others)
- `printer_base.cfg` - Base board configuration and common settings
- `beacon3d_probe.cfg` - Beacon3D probe configuration
- `dual_z_motors.cfg` - Dual Z motor configuration and macros
- `input_shaper.cfg` - Input shaper and resonance compensation
- `fan_control.cfg` - Fan control configuration and macros
- `leveling.cfg` - Bed leveling and Z calibration macros
- `pid_tuning.cfg` - Temperature control and PID tuning macros
- `testing.cfg` - Testing and diagnostic macros
- `macros.cfg` - Print management and utility macros

## Macro-Based Organization

This configuration uses a modular, macro-based approach for better organization and easier maintenance:

### Fan Control (`fan_control.cfg`)
- `FAN_ON`, `FAN_OFF`, `FAN_TOGGLE` - Basic fan control
- `FAN_SPEED`, `FAN_PERCENT` - Precise fan speed control
- `AUTO_FAN` - Automatic fan control based on layer
- `FAN_TEST`, `FAN_STATUS` - Testing and status

### Leveling (`leveling.cfg`)
- `LEVEL_BED`, `QUICK_LEVEL` - Bed leveling procedures
- `Z_OFFSET_CALIBRATE`, `Z_OFFSET_ADJUST` - Z offset calibration
- `Z_OFFSET_UP`, `Z_OFFSET_DOWN` - Quick Z offset adjustments
- `BED_MESH_CALIBRATE`, `BED_MESH_STATUS` - Bed mesh operations
- `LEVEL_TEST`, `LEVEL_VISUAL` - Leveling tests

### PID Tuning (`pid_tuning.cfg`)
- `PID_TUNE_HOTEND`, `PID_TUNE_BED`, `PID_TUNE_ALL` - PID tuning
- `HEAT_UP`, `COOL_DOWN` - Temperature control
- `SET_TEMP`, `WAIT_TEMP` - Temperature setting and waiting
- `TEMP_STATUS`, `PID_STATUS` - Status information
- `TEMP_TEST`, `TEMP_GRAPH` - Temperature testing

### Testing (`testing.cfg`)
- `TEST_ALL` - Comprehensive printer test
- `TEST_MOVEMENT`, `TEST_EXTRUSION` - Basic functionality tests
- `TEST_ACCELERATION`, `TEST_VELOCITY` - Performance tests
- `TEST_RESONANCE`, `TEST_PRESSURE_ADVANCE` - Calibration tests
- `TEST_ENDSTOPS`, `TEST_PROBE` - Hardware tests

### Print Management (`macros.cfg`)
- `START_PRINT`, `END_PRINT` - Print start/end procedures
- `PAUSE`, `RESUME`, `CANCEL_PRINT` - Print control
- `CALIBRATE_ALL` - Complete calibration procedure
- `STATUS`, `HELP` - Status and help information

## Hardware Setup

### BigTreeTech Octopus EX Max Pin Assignments

#### Stepper Motors
- **X Motor**: PB13 (step), PB12 (dir), PB14 (enable)
- **Y Motor**: PB10 (step), PB2 (dir), PB11 (enable)
- **Z Motor 1 (Left)**: PB0 (step), PC5 (dir), PB1 (enable)
- **Z Motor 2 (Right)**: PA15 (step), PA8 (dir), PC6 (enable)
- **Extruder**: PB3 (step), PB4 (dir), PD1 (enable)

#### Endstops
- **X Endstop**: PC0
- **Y Endstop**: PC1
- **Z Endstop**: Virtual (Beacon3D probe)

#### Heaters and Sensors
- **Hotend Heater**: PA1
- **Hotend Thermistor**: PC4
- **Bed Heater**: PA0
- **Bed Thermistor**: PC3

#### Fans
- **Part Cooling Fan**: PA8
- **Heatbreak Fan**: PE5
- **Controller Fan**: PE4

#### Beacon3D Probe
- **Probe Pin**: PC2
- **UART Pin**: PC7

#### Other
- **Beeper**: PA13

### TMC2209 Driver Configuration
All stepper drivers are configured as TMC2209 with UART communication:
- **UART Pin**: PC1
- **Addresses**: X=0, Y=2, Z=1, Z1=3, E=4

## Installation Instructions

### 1. Flash Klipper Firmware
1. Download the Klipper firmware for STM32F446
2. Flash to your Octopus EX Max board
3. Note the serial port (usually `/dev/serial/by-id/usb-Klipper_stm32f446xx_*`)

### 2. Update Configuration
1. Copy all configuration files to your Klipper config directory
2. Update the serial port in `printer_base.cfg`:
   ```ini
   [mcu]
   serial: /dev/serial/by-id/usb-Klipper_stm32f446xx_*
   ```

### 3. Calibrate Your Printer

#### Initial Setup
1. **Home all axes**: `G28`
2. **Test movement**: `TEST_MOVEMENT`
3. **Test all systems**: `TEST_ALL`

#### Z Offset Calibration
1. **Calibrate Z offset**: `Z_OFFSET_CALIBRATE`
2. **Save configuration**: `SAVE_CONFIG`

#### Beacon3D Probe Setup
1. **Initialize probe**: `BEACON3D_INIT`
2. **Calibrate probe**: `BEACON3D_CALIBRATE`
3. **Create bed mesh**: `BED_MESH_CALIBRATE`

#### Input Shaper Calibration
1. **Calibrate input shaper**: `INPUT_SHAPER_CALIBRATE`
2. **Follow the generated test print instructions**
3. **Save configuration**: `SAVE_CONFIG`

#### PID Tuning
1. **Tune both heaters**: `PID_TUNE_ALL`
2. **Save configuration**: `SAVE_CONFIG`

## Key Macros by Category

### Print Management
- `START_PRINT TEMP=200 BED_TEMP=60` - Start a print
- `END_PRINT` - End a print
- `PAUSE` - Pause a print
- `RESUME TEMP=200` - Resume a print
- `CANCEL_PRINT` - Cancel a print

### Calibration
- `CALIBRATE_ALL` - Run all calibration procedures
- `LEVEL_BED` - Complete bed leveling
- `Z_OFFSET_CALIBRATE` - Calibrate Z offset
- `PID_TUNE_ALL` - PID tune both heaters

### Testing
- `TEST_ALL` - Comprehensive printer test
- `TEST_MOVEMENT` - Test all movement axes
- `TEST_EXTRUSION` - Test extrusion
- `TEST_RESONANCE` - Test resonance for input shaper

### Fan Control
- `FAN_ON SPEED=255` - Turn on fan at full speed
- `FAN_PERCENT PERCENT=50` - Set fan to 50%
- `AUTO_FAN LAYER=2` - Auto fan for layer 2
- `FAN_STATUS` - Show fan status

### Temperature Control
- `HEAT_UP HOTEND=200 BED=60` - Heat up both heaters
- `COOL_DOWN` - Turn off all heaters
- `SET_TEMP HOTEND=200` - Set hotend temperature
- `TEMP_STATUS` - Show temperature status

### Leveling
- `LEVEL_BED` - Complete bed leveling
- `QUICK_LEVEL` - Quick leveling (no mesh)
- `Z_OFFSET_UP` - Move Z offset up 0.01mm
- `Z_OFFSET_DOWN_10` - Move Z offset down 0.1mm

### Maintenance
- `CLEAN_NOZZLE` - Clean nozzle procedure
- `EMERGENCY_STOP` - Emergency stop procedure
- `STATUS` - Show printer status
- `HELP` - Show available macros

## Configuration Notes

### Step Distances
- **X/Y**: 0.0125 (80 steps/mm for 20T pulleys)
- **Z**: 0.04 (400 steps/mm for T8 lead screws)
- **Extruder**: 0.0025 (400 steps/mm for 1.75mm filament)

### Bed Size
- **X**: 235mm
- **Y**: 235mm
- **Z**: 250mm

### Acceleration and Speed
- **Max Velocity**: 300 mm/s
- **Max Acceleration**: 3000 mm/s²
- **Max Z Velocity**: 5 mm/s
- **Max Z Acceleration**: 100 mm/s²

## Troubleshooting

### Common Issues

#### Z Motors Not Synchronized
1. Run `EMERGENCY_Z_ALIGN`
2. Manually align the Z motors
3. Run `DUAL_Z_HOME`

#### Beacon3D Probe Issues
1. Check UART connection (PC7)
2. Verify probe pin (PC2)
3. Run `BEACON3D_INIT`
4. Check probe calibration

#### Input Shaper Issues
1. Run `INPUT_SHAPER_CALIBRATE`
2. Follow test print instructions
3. Update frequencies in `input_shaper.cfg`

#### TMC2209 Driver Issues
1. Check UART connections
2. Verify driver addresses
3. Check current settings
4. Test with `QUERY_UBID`

### Emergency Procedures
- **Emergency Stop**: `EMERGENCY_STOP`
- **Disable Steppers**: `M84`
- **Turn Off Heaters**: `COOL_DOWN`
- **Turn Off Fans**: `ALL_FANS_OFF`

## Customization

### Adding Additional Features
- **Filament Sensor**: Uncomment in `printer_base.cfg`
- **Display**: Uncomment in `printer_base.cfg`
- **LED Strips**: Uncomment in `printer_base.cfg`
- **Power Supply Control**: Uncomment in `printer_base.cfg`

### Modifying Pin Assignments
Update the pin assignments in `printer.cfg` and `printer_base.cfg` to match your specific wiring.

### Adjusting Mechanical Settings
- **Step distances**: Update in `printer.cfg`
- **Bed size**: Update in `printer.cfg` and `beacon3d_probe.cfg`
- **Acceleration**: Update in `printer.cfg`

### Creating Custom Macros
Add your custom macros to the appropriate configuration file:
- **Fan-related**: `fan_control.cfg`
- **Leveling-related**: `leveling.cfg`
- **Temperature-related**: `pid_tuning.cfg`
- **Testing-related**: `testing.cfg`
- **General utilities**: `macros.cfg`

## Support

For issues specific to:
- **BigTreeTech Octopus EX Max**: Check the [official documentation](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-EX-MAX)
- **Beacon3D Probe**: Check the [official documentation](https://beacon3d.com/)
- **Klipper**: Check the [official documentation](https://www.klipper3d.org/)

## License

This configuration is provided as-is for educational and personal use. Modify as needed for your specific setup. 