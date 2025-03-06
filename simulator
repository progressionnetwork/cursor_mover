import pyautogui
import random
import time
import subprocess
import sys
import argparse
import math

def find_firefox_windows():
    """Find Firefox windows based on the operating system"""
    firefox_windows = []
    
    try:
        if sys.platform == "win32":
            # Windows: Use pyautogui to find windows with Firefox in their title
            windows = pyautogui.getAllWindows()
            firefox_windows = [w for w in windows if "Firefox" in w.title]
        
        elif sys.platform == "darwin":
            # macOS: Use AppleScript to find Firefox windows
            script = """
            tell application "System Events"
                if process "Firefox" exists then
                    tell process "Firefox"
                        get position and size of windows
                    end tell
                end if
            end tell
            """
            result = subprocess.run(['osascript', '-e', script], capture_output=True, text=True)
            if result.stdout.strip():
                # Parse the output to get window positions and sizes
                # This is simplified - actual parsing would depend on output format
                firefox_windows = [{'title': 'Firefox'}]  # Placeholder
        
        elif sys.platform.startswith("linux"):
            # Linux: Use xdotool to find Firefox windows
            try:
                result = subprocess.run(
                    ['xdotool', 'search', '--name', 'Firefox'], 
                    capture_output=True, 
                    text=True
                )
                if result.stdout.strip():
                    firefox_windows = [{'title': 'Firefox'}]  # Placeholder
            except FileNotFoundError:
                print("xdotool not found. Please install it with: sudo apt-get install xdotool")
    
    except Exception as e:
        print(f"Error detecting Firefox windows: {e}")
    
    return firefox_windows

def get_screen_dimensions():
    """Get the screen dimensions"""
    return pyautogui.size()

def is_inside_firefox(x, y, firefox_windows):
    """Check if coordinates are inside any Firefox window"""
    if not firefox_windows:
        # If we couldn't detect Firefox windows, assume whole screen
        return True
        
    # For simplicity in this example:
    # Since detailed window bounds detection depends on the platform and requires
    # more complex implementation, we'll assume coordinates are valid if Firefox is running
    return True

def random_movement_patterns(duration_minutes=60, min_move_interval=5, max_move_interval=30):
    """
    Move the mouse cursor in random patterns to simulate user activity
    
    Parameters:
    - duration_minutes: How long to run the simulation (in minutes)
    - min_move_interval: Minimum time between movements (in seconds)
    - max_move_interval: Maximum time between movements (in seconds)
    """
    print(f"Starting mouse movement simulation for {duration_minutes} minutes...")
    print("Press Ctrl+C to stop")
    
    # Find Firefox windows
    firefox_windows = find_firefox_windows()
    if not firefox_windows:
        print("No Firefox windows detected. Will move cursor across the entire screen.")
    else:
        print(f"Detected {len(firefox_windows)} Firefox windows")
    
    screen_width, screen_height = get_screen_dimensions()
    
    # Get current cursor position as starting point
    last_x, last_y = pyautogui.position()
    
    # Calculate end time
    end_time = time.time() + (duration_minutes * 60)
    
    # Movement patterns
    patterns = [
        "direct",       # Direct movement to target
        "smooth",       # Smooth curved movement
        "zigzag",       # Zigzag pattern
        "spiral",       # Spiral-like movement
        "human_like"    # Try to mimic human-like mouse movement with slight jitter
    ]
    
    try:
        while time.time() < end_time:
            # Choose a random target position within the screen bounds
            target_x = random.randint(0, screen_width - 1)
            target_y = random.randint(0, screen_height - 1)
            
            # Check if target is inside Firefox window (or use whole screen if windows not detected)
            if is_inside_firefox(target_x, target_y, firefox_windows):
                # Select a random movement pattern
                pattern = random.choice(patterns)
                
                # Execute the selected movement pattern
                if pattern == "direct":
                    # Direct movement
                    pyautogui.moveTo(target_x, target_y, duration=random.uniform(0.2, 1.0))
                
                elif pattern == "smooth":
                    # Smooth curved movement using Bézier curve approximation
                    steps = random.randint(20, 50)
                    current_x, current_y = pyautogui.position()
                    
                    # Control point for the curve
                    control_x = random.randint(min(current_x, target_x), max(current_x, target_x))
                    control_y = random.randint(min(current_y, target_y), max(current_y, target_y))
                    
                    for i in range(1, steps + 1):
                        t = i / steps
                        # Quadratic Bézier curve formula
                        x = (1-t)**2 * current_x + 2*(1-t)*t * control_x + t**2 * target_x
                        y = (1-t)**2 * current_y + 2*(1-t)*t * control_y + t**2 * target_y
                        pyautogui.moveTo(x, y, duration=random.uniform(0.005, 0.02))
                
                elif pattern == "zigzag":
                    # Zigzag pattern
                    steps = random.randint(3, 8)
                    current_x, current_y = pyautogui.position()
                    
                    step_x = (target_x - current_x) / steps
                    step_y = (target_y - current_y) / steps
                    
                    for i in range(1, steps + 1):
                        mid_x = current_x + step_x * i
                        mid_y = current_y + step_y * i
                        # Add zigzag offset to y coordinate
                        offset = random.randint(10, 30) * (-1 if i % 2 == 0 else 1)
                        pyautogui.moveTo(mid_x, mid_y + offset, duration=random.uniform(0.1, 0.3))
                    
                    # Finally move to the exact target
                    pyautogui.moveTo(target_x, target_y, duration=random.uniform(0.1, 0.3))
                
                elif pattern == "spiral":
                    # Spiral-like movement
                    current_x, current_y = pyautogui.position()
                    center_x = (current_x + target_x) / 2
                    center_y = (current_y + target_y) / 2
                    
                    # Calculate max radius based on distance
                    max_radius = min(
                        abs(target_x - current_x), 
                        abs(target_y - current_y),
                        100  # Maximum spiral radius
                    ) / 2
                    
                    # Create a spiral path
                    steps = random.randint(15, 30)
                    for i in range(steps):
                        angle = i * math.pi / 8
                        radius = max_radius * i / steps
                        x = center_x + radius * math.cos(angle)
                        y = center_y + radius * math.sin(angle)
                        pyautogui.moveTo(x, y, duration=random.uniform(0.01, 0.05))
                    
                    # Finally move to the exact target
                    pyautogui.moveTo(target_x, target_y, duration=random.uniform(0.1, 0.3))
                
                elif pattern == "human_like":
                    # Human-like movement with slight jitter and variable speed
                    current_x, current_y = pyautogui.position()
                    distance = math.sqrt((target_x - current_x)**2 + (target_y - current_y)**2)
                    
                    # Adjust steps based on distance - longer distances need more steps
                    steps = int(max(10, min(50, distance / 10)))
                    
                    for i in range(1, steps + 1):
                        # Calculate progress along path with slight acceleration and deceleration
                        t = i / steps
                        # Ease-in and ease-out effect
                        easing = math.sin(t * math.pi / 2) if t < 0.5 else 1 - math.sin((1-t) * math.pi / 2)
                        
                        # Calculate position with slight random jitter
                        jitter_x = random.uniform(-2, 2) if i > 1 and i < steps else 0
                        jitter_y = random.uniform(-2, 2) if i > 1 and i < steps else 0
                        
                        x = current_x + (target_x - current_x) * easing + jitter_x
                        y = current_y + (target_y - current_y) * easing + jitter_y
                        
                        # Variable speed - slower at start and end
                        duration = random.uniform(0.01, 0.03)
                        if i < steps * 0.2 or i > steps * 0.8:
                            duration *= 1.5
                        
                        pyautogui.moveTo(x, y, duration=duration)
                
                print(f"Moved cursor to ({target_x}, {target_y}) using {pattern} pattern")
                
                # Sometimes perform a click (5% chance)
                if random.random() < 0.05:
                    pyautogui.click()
                    print("Clicked!")
                
                # Sometimes perform a scroll (10% chance)
                if random.random() < 0.1:
                    scroll_amount = random.randint(-5, 5)
                    pyautogui.scroll(scroll_amount)
                    print(f"Scrolled: {scroll_amount}")
                
                # Sometimes perform a right-click (2% chance)
                if random.random() < 0.02:
                    pyautogui.rightClick()
                    print("Right-clicked!")
                    # After right-click, move away from the context menu and left-click to dismiss
                    time.sleep(random.uniform(0.5, 1.5))
                    offset_x = random.randint(100, 200) * random.choice([-1, 1])
                    offset_y = random.randint(100, 200) * random.choice([-1, 1])
                    pyautogui.moveRel(offset_x, offset_y, duration=random.uniform(0.2, 0.5))
                    pyautogui.click()
            
            # Wait for a random interval before the next movement
            wait_time = random.uniform(min_move_interval, max_move_interval)
            print(f"Waiting for {wait_time:.2f} seconds...")
            time.sleep(wait_time)
            
    except KeyboardInterrupt:
        print("\nSimulation stopped by user")
    
    print("Mouse movement simulation completed")

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Simulate mouse movements for Firefox")
    parser.add_argument("--duration", type=int, default=60, 
                        help="Duration in minutes (default: 60)")
    parser.add_argument("--min-interval", type=float, default=5, 
                        help="Minimum seconds between movements (default: 5)")
    parser.add_argument("--max-interval", type=float, default=30, 
                        help="Maximum seconds between movements (default: 30)")
    
    args = parser.parse_args()
    
    # Ensure PyAutoGUI has a failsafe mechanism
    pyautogui.FAILSAFE = True
    print("IMPORTANT: Move the mouse to the top-left corner of the screen to abort the script (failsafe mechanism)")
    
    random_movement_patterns(args.duration, args.min_interval, args.max_interval)
