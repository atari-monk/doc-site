# Metronome

**Question:** Py script to simulate metronome

Here’s a Python script to simulate a metronome:



```python

import time

import argparse



def metronome(bpm: int, beats: int):

    """

    Simulate a metronome.

    

    Args:

        bpm (int): Beats per minute.

        beats (int): Number of beats to simulate.

    """

    interval = 60.0 / bpm

    print(f"Metronome started: {bpm} BPM, {beats} beats")

    for beat in range(1, beats + 1):

        print(f"Beat {beat}")

        time.sleep(interval)

    print("Metronome stopped.")



if __name__ == "__main__":

    parser = argparse.ArgumentParser(description="Metronome Simulator")

    parser.add_argument("--bpm", type=int, default=60, help="Beats per minute (default: 60)")

    parser.add_argument("--beats", type=int, default=16, help="Number of beats (default: 16)")

    args = parser.parse_args()



    try:

        metronome(args.bpm, args.beats)

    except KeyboardInterrupt:

        print("\nMetronome stopped manually.")

```



### How to Use

1. Save this script to a file (e.g., `metronome.py`).

2. Run it from the terminal or command line:

   ```bash

   python metronome.py --bpm 120 --beats 8

   ```

   Replace `120` with your desired BPM and `8` with the number of beats to simulate. 



3. Press `Ctrl+C` to stop the metronome manually.
