# Custom CoreXY Printer Configuration
## 400x400x400 Print Volume with Dual Z and Beacon3D Probe
## High-Speed Optimized with 17HS24-2104S & 17HE24-2104S Motors

This configuration is adapted from an existing Ender 3-like setup for a custom CoreXY printer with the following specifications:

### Printer Specifications
- **Kinematics**: CoreXY
- **Print Volume**: 400x400x400mm
- **Mainboard**: BigTreeTech Octopus EX Max
- **Probe**: Beacon3D Eddy Current Probe
- **Z-Axis**: Dual Z motors (synchronized)
- **Stepper Drivers**: TMC2209 (UART)
- **Motors**: 17HS24-2104S (X/Y/E) & 17HE24-2104S (Z1/Z2)
- **Max Speed**: 500mm/s
- **Max Acceleration**: 5000mm/s²

### Motor Specifications
Based on datasheets from [https://github.com/JCorellaFSL/NecroCubicon](https://github.com/JCorellaFSL/NecroCubicon):

#### 17HS24-2104S (CoreXY & Extruder)
- **Step angle**: 1.8° (200 steps/rev)
- **Rated current**: 2.1A
- **Rated voltage**: 3.6V
- **Resistance**: 1.7Ω
- **Inductance**: 3.2mH
- **Holding torque**: 0.4N⋅m
- **Detent torque**: 0.02N⋅m
- **Rotor inertia**: 54g⋅cm²
- **Weight**: 280g

#### 17HE24-2104S (Dual Z Motors)
- **Step angle**: 1.8° (200 steps/rev)
- **Rated current**: 2.1A
- **Rated voltage**: 3.6V
- **Resistance**: 1.7Ω
- **Inductance**: 3.2mH
- **Holding torque**: 0.4N⋅m
- **Detent torque**: 0.02N⋅m
- **Rotor inertia**: 54g⋅cm²
- **Weight**: 280g

### Configuration Files

#### Main Configuration Files
- `printer.cfg` - Main configuration file (high-speed optimized)
- `printer_base.cfg` - Base board and communication settings
- `motor_specs.cfg` - Motor specifications and high-speed TMC2209 settings
- `corexy_kinematics.cfg` - CoreXY-specific macros and settings
- `beacon3d_probe.cfg` - Beacon3D probe configuration
- `dual_z_motors.cfg` - Dual Z motor synchronization

#### Supporting Files
- `macros.cfg` - Print management and utility macros
- `input_shaper.cfg` - Input shaper configuration
- `fan_control.cfg` - Fan control settings
- `leveling.cfg` - Bed leveling procedures
- `pid_tuning.cfg` - PID tuning macros
- `testing.cfg` - Testing and calibration procedures

### Key Changes from Original Configuration

#### 1. Kinematics Change
- Changed from `cartesian` to `corexy` kinematics
- Updated stepper names from `stepper_x`/`stepper_y` to `stepper_a`/`stepper_b`
- Maintained same pin assignments for compatibility

#### 2. Print Volume Updates
- Updated all position limits from 235x235x250 to 400x400x400
- Adjusted bed mesh settings for larger bed
- Updated calibration points for 400x400 bed

#### 3. High-Speed Optimizations
- **Max Velocity**: Increased to 500mm/s
- **Max Acceleration**: Increased to 5000mm/s²
- **Motor Currents**: Optimized for 17HS24-2104S & 17HE24-2104S
- **Input Shaper**: Tuned for higher frequencies (60Hz)
- **Homing Speeds**: Increased for faster operation

#### 4. Motor-Specific Tuning
- **CoreXY Motors**: 1.2A run current, 0.6A hold current
- **Z Motors**: 1.0A run current, 0.5A hold current
- **Extruder**: 0.8A run current, 0.4A hold current
- **TMC2209 Settings**: Optimized for high-speed operation

#### 5. CoreXY-Specific Features
- Added CoreXY homing sequence
- Added belt tensioning macros
- Added frame squaring assistance
- Added CoreXY movement testing
- Added performance profiles (SPEED, BALANCED, QUALITY)

### Initial Setup and Calibration

#### 1. Hardware Setup
1. Ensure CoreXY belts are properly tensioned
2. Verify dual Z motors are mechanically aligned
3. Check Beacon3D probe is properly mounted
4. Verify all endstops are correctly positioned
5. Ensure proper motor wiring for 17HS24-2104S & 17HE24-2104S

#### 2. Initial Configuration
1. Update the MCU serial path in `printer_base.cfg` if needed
2. Verify TMC2209 UART addresses match your wiring
3. Check endstop pin assignments
4. Verify motor current settings match your power supply

#### 3. Calibration Sequence
Run the following commands in order:

```gcode
; 1. Home all axes
COREXY_HOME

; 2. Set performance profile
SET_PERFORMANCE_PROFILE PROFILE=BALANCED

; 3. Calibrate Z offset
Z_OFFSET_CALIBRATE

; 4. Perform Z tilt adjustment
Z_TILT_ADJUST

; 5. Create bed mesh
BED_MESH_CALIBRATE

; 6. Save configuration
SAVE_CONFIG
```

### Important Macros

#### High-Speed Macros
- `OPTIMIZE_FOR_SPEED` - Optimize all settings for high-speed printing
- `MOTOR_TUNE_HIGH_SPEED` - Tune motors for high-speed operation
- `TEST_HIGH_SPEED` - Test high-speed movement patterns
- `SET_PERFORMANCE_PROFILE` - Set SPEED/BALANCED/QUALITY profiles
- `MOTOR_STATUS` - Show motor status and temperatures
- `MOTOR_TEMP_CHECK` - Check motor temperatures with warnings

#### CoreXY-Specific Macros
- `COREXY_HOME` - Home all axes using CoreXY kinematics
- `COREXY_LEVEL` - Complete bed leveling procedure
- `COREXY_TEST_MOVEMENT` - Test CoreXY movement patterns
- `COREXY_TENSION_BELTS` - Belt tensioning assistance
- `COREXY_SQUARE_FRAME` - Frame squaring assistance

#### Print Management
- `START_PRINT` - Start print with proper homing and leveling
- `END_PRINT` - End print and park toolhead
- `PAUSE`/`RESUME` - Pause and resume print functionality

#### Calibration
- `CALIBRATE_ALL` - Run complete calibration sequence
- `Z_OFFSET_CALIBRATE` - Calibrate Z offset with dual Z
- `PID_TUNE_ALL` - PID tune both heaters

### Performance Profiles

The configuration includes three performance profiles:

#### SPEED Profile
- **Velocity**: 500mm/s
- **Acceleration**: 5000mm/s²
- **Motor Currents**: Maximum (1.2A CoreXY, 1.0A Z, 0.8A Extruder)
- **Use Case**: Fast prototyping, draft prints

#### BALANCED Profile (Default)
- **Velocity**: 300mm/s
- **Acceleration**: 3000mm/s²
- **Motor Currents**: Balanced (1.0A CoreXY, 0.8A Z, 0.7A Extruder)
- **Use Case**: General printing, good quality/speed balance

#### QUALITY Profile
- **Velocity**: 200mm/s
- **Acceleration**: 2000mm/s²
- **Motor Currents**: Lower (0.8A CoreXY, 0.6A Z, 0.6A Extruder)
- **Use Case**: High-quality prints, detailed models

### Bed Mesh Configuration

The bed mesh is configured for a 400x400 bed with:
- Mesh minimum: 20, 20
- Mesh maximum: 380, 380
- Probe count: 5x5
- Algorithm: bicubic

### Dual Z Configuration

The dual Z motors are synchronized with:
- Z tilt adjustment points at corners and center
- Automatic synchronization during homing
- Emergency alignment procedures if motors get out of sync

### Beacon3D Probe

The Beacon3D eddy current probe is configured with:
- UART communication on pin PC7
- Virtual Z endstop for homing
- Automatic calibration procedures
- Bed mesh creation support

### High-Speed Considerations

#### Motor Temperature Monitoring
- Monitor motor temperatures during high-speed operation
- Use `MOTOR_TEMP_CHECK` to check temperatures
- Motors should stay below 80°C for optimal performance

#### Belt Tension
- Proper belt tension is critical for high-speed operation
- Use `COREXY_TENSION_BELTS` to check tension
- Belts should be tight but not over-tensioned

#### Frame Rigidity
- Ensure frame is square and rigid
- Use `COREXY_SQUARE_FRAME` to check squareness
- Consider frame reinforcements for high-speed operation

#### Input Shaper Calibration
- Calibrate input shaper for your specific frame
- Higher frequencies (60Hz) are set for high-speed operation
- Adjust based on your frame's resonance characteristics

### Troubleshooting

#### Common Issues

1. **Motors moving in wrong direction**
   - Check `dir_pin` settings in stepper configurations
   - Verify CoreXY belt routing

2. **Z motors out of sync**
   - Run `EMERGENCY_Z_ALIGN` macro
   - Manually align motors and re-home

3. **Probe not working**
   - Check UART connection to Beacon3D
   - Verify probe offset settings
   - Run `BEACON3D_CALIBRATE`

4. **Poor print quality at high speeds**
   - Run input shaper calibration
   - Check belt tension with `COREXY_TENSION_BELTS`
   - Verify frame squareness with `COREXY_SQUARE_FRAME`
   - Monitor motor temperatures with `MOTOR_TEMP_CHECK`

5. **Motor overheating**
   - Reduce motor currents using performance profiles
   - Check for mechanical binding
   - Ensure proper cooling

#### Testing Procedures

1. **Movement Test**
   ```gcode
   COREXY_TEST_MOVEMENT
   ```

2. **High-Speed Test**
   ```gcode
   TEST_HIGH_SPEED
   ```

3. **Belt Tension Check**
   ```gcode
   COREXY_TENSION_BELTS
   ```

4. **Motor Temperature Check**
   ```gcode
   MOTOR_TEMP_CHECK
   ```

### Slicer Configuration

Use the provided slicer start and end gcode files:
- `slicer_start_gcode.txt` - Start gcode for CoreXY
- `slicer_end_gcode.txt` - End gcode for CoreXY

### Safety Notes

1. Always home the printer before starting prints
2. Check Z offset after any mechanical changes
3. Monitor dual Z synchronization during long prints
4. Keep Beacon3D probe clean and calibrated
5. Monitor motor temperatures during high-speed operation
6. Start with BALANCED profile and gradually increase to SPEED profile

### Performance Optimization

1. **Input Shaper**: Calibrate for your specific frame resonance
2. **Pressure Advance**: Tune for your extruder and filament
3. **Speed Settings**: Adjust based on your frame stiffness and motor temperatures
4. **Bed Mesh**: Create and save bed mesh profiles for different bed surfaces
5. **Performance Profiles**: Use appropriate profile for your print requirements

### Maintenance

Regular maintenance tasks:
1. Check belt tension monthly
2. Calibrate Z offset weekly
3. Create new bed mesh when changing bed surface
4. Clean Beacon3D probe as needed
5. Check dual Z alignment monthly
6. Monitor motor temperatures during high-speed operation
7. Check frame squareness quarterly

### Support

For issues specific to this CoreXY configuration:
1. Check the troubleshooting section above
2. Verify all pin assignments match your wiring
3. Test individual components using the provided test macros
4. Consult the original Klipper documentation for advanced configuration
5. Check motor datasheets in the [GitHub repository](https://github.com/JCorellaFSL/NecroCubicon)

---

**Note**: This configuration assumes the same BigTreeTech Octopus EX Max pin assignments as the original setup. Adjust pin assignments if your wiring differs. Motor specifications are based on the datasheets in the [NecroCubicon repository](https://github.com/JCorellaFSL/NecroCubicon). 