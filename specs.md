You are a highly skilled senior architect for robotics and firmware and web development.
You are expected to perform and help me with my quadrupet robot pet project using these components:

1. ESP32 DEVKIT V1
2. SSD1306
3. MPU6050
4. 12 MG90S Metal Gear Servos
5. PCA9865

SSD1306, MPU6050, and PCA9865 are all connected to I2C in the pin number:
D21 - SDA
D22 - SCL

and will be supporting some more soon. For now, I am exploring to learn Inverse Kinematics

# And these are the parametrs and formulas

### Make sure you follow the XYZ orientation
- Y = Vertical (Up/Down)
- X = Depth (Forward/Backward)
- Z = Lateral (Left/Right)

### XYZ Orientation
```
             Y+
             |
             |
             |
X+ __________|__________ -X
              \
               \
                \
                 Z+
```

- +X is where the head of the robot is
- -Y is there the tail is
- Z+ is where the left side of the robot is

### Constants:
- A = Coxa Length (28mm)
- F = Femur Length (50mm)
- T = Tibia Length (72mm)

### Front visualization of the leg:
```
COXA    FEMUR
O---------O
          |
          |
          O TIBIA
          |
          |
          |
```

### Front Left (Side visualization) of the leg
```
FEMUR    TIBIA
O----------O
          /
         /
        /
       / 
     _/ FEET
```

### Inputs:
- Y = Vertical (Up/Down)
- X = Depth (Forward/Backward)
- Z = Lateral (Left/Right)

## Inverse Kinematics Formula
### // --- 1. COXA Angle (ω) ---
- C = √(Z² + Y²)
- D = √(C² - A²)
### // Using atan2(opposite, adjacent)
- α_coxa = atan2(Z, Y)
- β_coxa = atan2(D, A)
- ω = α_coxa + β_coxa
### // --- 2. TIBIA Angle (φ) ---
- G = √(D² + X²)
### // Using Law of Cosines
- φ = acos((G² - F² - T²) / (-2 * F * T))
### // --- 3. FEMUR Angle (θ) ---
- α_femur = atan2(X, D)
### // Using Law of Sines
- β_femur = asin((T * sin(φ)) / G)
- θ = α_femur + β_femur  )  
- θ (Femur Angle) = atan(X / D) + asin((F * sin(φ)) / G)  
  
# Initial Implementation
Develop a working implementation of the Inverse Kinematics that will the robot's firmware. 

# Web Control
1. A 3D visualization of the leg that applies the same Inverse Kinematics formula. 
2. If I click on a joint, Coxa or Femur, or Tibia, it will give me a 3d visualization of a way to rotate in X Y or Z.
3. A visualization of the MPU6050 raw data
4. I should be able to adjust the visualization of the leg dragging the feet and the X, Y, Z position will also be sent in the firmware via WiFi.
5. I should be able to connect to a WiFi with an indicator if I am already connected or not.
6. I should be able to run the web using this command: `npm run dev`

# Firmware
1. Implement the Inverse Kinematics by the given formula
2. In the OLED, it should the current IP address

# Leg Map to PCA3685 pins
Location: COXA, FEMUR, TIBIA
Front Left: 0, 1, 2
Front Right: 4, 5, 6
Back Left: 8, 9, 10
Back Right: 11, 12, 13

# Initial Servo Positions
COXA: 90 degrees
FEMUR: 45 degrees
TIBIA: 45 degrees  