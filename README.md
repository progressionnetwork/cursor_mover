# cursor_mover
A Python script to randomly move the cursor to simulate user activity, specifically targeting Firefox windows.

Here's a Python script to randomly move the cursor to simulate user activity, specifically targeting Firefox windows:

To use this script:

1. You'll need to install the required libraries:
   ```
   pip install pyautogui
   ```

2. Save the script as `firefox_activity_simulator.py`

3. Run it from the command line:
   ```
   python simulator.py
   ```

4. Optional arguments:
   - `--duration` - How many minutes to run the simulation (default: 60)
   - `--min-interval` - Minimum seconds between movements (default: 5)
   - `--max-interval` - Maximum seconds between movements (default: 30)

   Example:
   ```
   python firefox_activity_simulator.py --duration 120 --min-interval 2 --max-interval 15
   ```

Features of this script:
1. Attempts to detect Firefox windows (though window boundary detection is simplified)
2. Uses 5 different movement patterns to simulate natural human behavior:
   - Direct movement
   - Smooth curved movement
   - Zigzag pattern
   - Spiral movement
   - Human-like movement with jitter and variable speed
3. Occasionally performs clicks, right-clicks, and scrolling
4. Includes a failsafe (move cursor to top-left corner to abort)
5. Configurable duration and timing intervals

Note: The window detection functionality might need adjustments based on your specific operating system and environment.

Would you like me to explain or break down the code?
