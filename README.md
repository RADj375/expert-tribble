# expert-tribble
Completed String Theory In Python
import math
import numpy as np
import matplotlib.pyplot as plt

class String:
    def __init__(self, length, tension):
        """Creates a string with the given length and tension."""
        self.length = length
        self.tension = tension
        self.points = []

    def calculate_displacement(self, x):
        """Calculates the displacement of the string at the given position x."""
        k = math.sqrt(self.tension / self.length)
        return math.sin(k * x)

    def update_points(self, dt):
        """Updates the positions of the points on the string."""
        for i in range(1, len(self.points) - 1):
            y_prev = self.points[i - 1]
            y_curr = self.points[i]
            y_next = self.points[i + 1]

            acceleration = self.tension / self.length * (y_prev - 2 * y_curr + y_next)
            velocity = self.points[i].velocity + acceleration * dt
            self.points[i].velocity = velocity
            self.points[i].position += velocity * dt

def main():
    """Simulates string theory."""
    # Create a string.
    length = 1.0
    tension = 1.0
    string = String(length, tension)

    # Initialize the points on the string.
    num_points = 100
    step_size = length / (num_points - 1)
    for x in range(num_points):
        position = np.array([x * step_size, 0.0])
        velocity = np.array([0.0, 0.0])
        string.points.append(position, velocity)

    # Simulate the string for a given time interval.
    dt = 0.01
    time_interval = 1.0
    for t in range(int(time_interval / dt)):
        # Update the positions of the points on the string.
        string.update_points(dt)

        # Plot the string.
        plt.clf()
        for point in string.points:
            plt.plot([point.position[0]], [point.position[1]], 'o')
        plt.xlim(0, length)
        plt.ylim(-1, 1)
        plt.pause(dt)

    # Wait for the user to press a key before exiting.
    input()

if __name__ == '__main__':
    main()
