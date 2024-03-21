INSSLAMGNSS

Accmeter is an Acceleretometer Application made with  Haskell.

# About
An accelerometer in a [GNSS/INS](#) system measures both the system's linear acceleration due to motion and the pseudo-acceleration caused by gravity.
In order to obtain the system's linear acceleration due to motion, the pseudo-acceleration caused by gravity must be subtracted from the accelerometer measurement using estimates of the system's attitude. The resulting linear acceleration measurement can then be integrated once to obtain the system's velocity and twice to obtain the system's position.
However, these calculations are heavily dependent on the INS maintaining an accurate attitude estimate, as any error in the attitude causes an error in the calculated acceleration, consequently causing errors in the integrated position and velocity.


# Develop Requirements
1. Haskell
2. 

# Build Requirements
1. Basel 

# Testing
