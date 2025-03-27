import tkinter as tk
from tkinter import ttk, messagebox

class Process:
    def __init__(self, pid, arrival_time, burst_time):
        self.pid = pid
        self.arrival_time = arrival_time
        self.burst_time = burst_time
        self.remaining_time = burst_time
        self.start_time = -1
        self.finish_time = 0
        self.waiting_time = 0
        self.turnaround_time = 0
        self.response_time = -1

def round_robin(processes, quantum):
    time = 0
    queue = []
    completed_processes = []
    processes.sort(key=lambda p: p.arrival_time)
    
    while processes or queue:
        while processes and processes[0].arrival_time <= time:
            queue.append(processes.pop(0))
        
        if queue:
            process = queue.pop(0)
            if process.response_time == -1:
                process.response_time = time - process.arrival_time
            
            execution_time = min(quantum, process.remaining_time)
            process.remaining_time -= execution_time
            time += execution_time
            
            while processes and processes[0].arrival_time <= time:
                queue.append(processes.pop(0))
            
            if process.remaining_time > 0:
                queue.append(process)
            else:
                process.finish_time = time
                process.turnaround_time = process.finish_time - process.arrival_time
                process.waiting_time = process.turnaround_time - process.burst_time
                completed_processes.append(process)
        else:
            time += 1
    
    return completed_processes

class RoundRobinApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Round-Robin Scheduler")
        
        self.process_list = []
        
        ttk.Label(root, text="Quantum:").grid(row=0, column=0)
        self.quantum_entry = ttk.Entry(root)
        self.quantum_entry.grid(row=0, column=1)
        
        ttk.Label(root, text="PID").grid(row=1, column=0)
        ttk.Label(root, text="Arrival Time").grid(row=1, column=1)
        ttk.Label(root, text="Burst Time").grid(row=1, column=2)
        
        self.entries = []
        for i in range(6):
            pid_entry = ttk.Entry(root, width=5)
            at_entry = ttk.Entry(root, width=10)
            bt_entry = ttk.Entry(root, width=10)
            
            pid_entry.grid(row=i+2, column=0)
            at_entry.grid(row=i+2, column=1)
            bt_entry.grid(row=i+2, column=2)
            
            self.entries.append((pid_entry, at_entry, bt_entry))
        
        self.run_button = ttk.Button(root, text="Run", command=self.run_scheduler)
        self.run_button.grid(row=8, column=1)
        
        self.result_text = tk.Text(root, height=10, width=50)
        self.result_text.grid(row=9, column=0, columnspan=3)
    
    def run_scheduler(self):
        try:
            quantum = int(self.quantum_entry.get())
            self.process_list = []
            
            for pid_entry, at_entry, bt_entry in self.entries:
                if pid_entry.get() and at_entry.get() and bt_entry.get():
                    self.process_list.append(Process(
                        pid_entry.get(), int(at_entry.get()), int(bt_entry.get())
                    ))
            
            if not self.process_list:
                messagebox.showerror("Error", "Please enter at least one process")
                return
            
            completed_processes = round_robin(self.process_list, quantum)
            
            self.result_text.delete("1.0", tk.END)
            self.result_text.insert(tk.END, "PID | Finish Time | Turnaround | Waiting\n")
            self.result_text.insert(tk.END, "-"*40 + "\n")
            
            for process in completed_processes:
                self.result_text.insert(tk.END, f"{process.pid} | {process.finish_time} | {process.turnaround_time} | {process.waiting_time}\n")
        except ValueError:
            messagebox.showerror("Error", "Invalid input. Please enter valid numbers.")

if __name__ == "__main__":
    root = tk.Tk()
    app = RoundRobinApp(root)
    root.mainloop()
