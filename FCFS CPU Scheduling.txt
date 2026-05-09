import random
import matplotlib.pyplot as plt
import pandas as pd

# Input
n = int(input("Enter number of processes: "))

processes = []

for i in range(n):
    processes.append({
        'PID': f'P{i+1}',
        'AT': random.randint(0, 10),
        'BT': random.randint(1, 10)
    })

# FCFS Scheduling
processes.sort(key=lambda x: x['AT'])

time = 0
total_wt = total_tat = 0
timeline = []

for p in processes:

    if time < p['AT']:
        timeline.append(('Idle', time, p['AT']))
        time = p['AT']

    p['Start'] = time
    p['CT'] = time + p['BT']
    p['TAT'] = p['CT'] - p['AT']
    p['WT'] = p['TAT'] - p['BT']

    total_wt += p['WT']
    total_tat += p['TAT']

    timeline.append((p['PID'], p['Start'], p['CT']))
    time = p['CT']

# Output
df = pd.DataFrame(processes)

print("\nFCFS Scheduling")
print(df[['PID', 'AT', 'BT', 'CT', 'TAT', 'WT']])

print(f"\nAverage WT  = {total_wt/n:.2f}")
print(f"Average TAT = {total_tat/n:.2f}")

# Gantt Chart
fig, ax = plt.subplots(figsize=(8, 4))

for i, (pid, start, end) in enumerate(timeline):

    color = 'red' if pid == 'Idle' else 'blue'

    ax.broken_barh([(start, end-start)], (i, 0.8), facecolors=color)

    ax.text((start+end)/2, i+0.4, pid,
            ha='center', va='center', color='white')

ax.set_xlabel("Time")
ax.set_ylabel("Processes")
ax.set_title("FCFS Gantt Chart")

plt.show()