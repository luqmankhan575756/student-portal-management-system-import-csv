<!-- attendance_readme.html -->
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Attendance Management — README demo</title>
  <style>
    :root{
      --bg:#0f1724; --card:#0b1220; --muted:#93a3b8; --accent:#06b6d4;
      --glass: rgba(255,255,255,0.03);
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    body{background:linear-gradient(180deg,#071029 0%, #021226 100%); color:#e6eef6; margin:0; padding:32px;}
    .container{max-width:980px;margin:0 auto;}
    header{display:flex;align-items:center;gap:16px;margin-bottom:18px;}
    h1{font-size:1.6rem;margin:0;}
    .badge{background:var(--glass);padding:6px 10px;border-radius:999px;color:var(--muted);font-size:0.9rem}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); border:1px solid rgba(255,255,255,0.03); padding:18px; border-radius:12px; box-shadow: 0 6px 30px rgba(2,6,23,0.6); margin-bottom:18px;}
    pre{background:#071226;padding:12px;border-radius:8px;overflow:auto;color:#d8f3ff}
    code{font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", "Noto Mono", monospace; font-size:0.88rem}
    .grid{display:grid;grid-template-columns:1fr 320px; gap:16px;}
    .muted{color:var(--muted);font-size:0.95rem}
    .btn{display:inline-block;padding:8px 12px;border-radius:8px;background:transparent;border:1px solid rgba(255,255,255,0.06);cursor:pointer}
    /* Demo UI */
    #demo form{display:flex;flex-direction:column;gap:8px}
    #demo input, #demo select{padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:inherit}
    table{width:100%;border-collapse:collapse}
    th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);text-align:left}
    .status-present{color:#10b981;font-weight:600}
    .status-absent{color:#ef4444;font-weight:600}
    footer{margin-top:20px;color:var(--muted);font-size:0.9rem}
    @media (max-width:880px){ .grid{grid-template-columns:1fr} }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div>
        <h1>Attendance Management System</h1>
        <div class="muted">Python + CSV command-line tool • Simple demo UI included</div>
      </div>
      <div style="margin-left:auto">
        <span class="badge">Python • CSV</span>
      </div>
    </header>

    <section class="card">
      <h2 style="margin-top:0">Project overview</h2>
      <p class="muted">
        A lightweight attendance manager that stores records to <code>attendance.csv</code>.
        Features: add attendance (name + timestamp + Present/Absent), view all records, search by name.
      </p>

      <div style="display:flex;gap:10px;margin-top:12px;flex-wrap:wrap">
        <div class="card" style="padding:12px;">
          <strong>Requirements</strong>
          <ul class="muted">
            <li>Python 3.7+</li>
            <li>No external dependencies (uses stdlib <code>csv</code> and <code>datetime</code>)</li>
          </ul>
        </div>

        <div class="card" style="padding:12px;">
          <strong>Quick start</strong>
          <ol class="muted">
            <li>Save the provided script as <code>attendance.py</code>.</li>
            <li>Run: <code>python attendance.py</code></li>
            <li>Follow the on-screen menu to add, view, or search attendance.</li>
          </ol>
        </div>
      </div>
    </section>

    <div class="grid">
      <section class="card" aria-labelledby="code-title">
        <h3 id="code-title">Python script (full)</h3>
        <p class="muted" style="margin-top:0">Copy the script below into <code>attendance.py</code>.</p>
        <pre><code>
import csv
from datetime import datetime


FILENAME = "attendance.csv"

def add_attendance():
    name = input("Enter student name: ").strip()
    status = input("Enter status (Present/Absent): ").strip().capitalize()
    date = datetime.now().strftime("%Y-%m-%d")

    
    with open(FILENAME, "a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([name, date, status])
    
    print(f"✅ Attendance marked for {name} on {date} as {status}")


def view_attendance():
    try:
        with open(FILENAME, "r") as file:
            reader = csv.reader(file)
            print("\n--- Attendance Record ---")
            for row in reader:
                print(row)
    except FileNotFoundError:
        print("⚠️ No attendance record found yet!")


def search_attendance():
    name = input("Enter student name to search: ").strip()
    found = False
    try:
        with open(FILENAME, "r") as file:
            reader = csv.reader(file)
            for row in reader:
                if row and row[0].lower() == name.lower():
                    print(row)
                    found = True
        if not found:
            print(f"❌ No record found for {name}")
    except FileNotFoundError:
        print("⚠️ No attendance file found!")


def main():
    while True:
        print("\n==== Attendance Management System ====")
        print("1. Add Attendance")
        print("2. View Attendance")
        print("3. Search Attendance by Name")
        print("4. Exit")
        
        choice = input("Enter your choice: ")

        if choice == "1":
            add_attendance()
        elif choice == "2":
            view_attendance()
        elif choice == "3":
            search_attendance()
        elif choice == "4":
            print("👋 Exiting program... Goodbye!")
            break
        else:
            print("❌ Invalid choice. Please try again.")


if __name__ == "__main__":
    
    try:
        with open(FILENAME, "x", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Name", "Date", "Status"])
    except FileExistsError:
        pass  

    main()
        </code></pre>
      </section>

      <aside class="card">
        <h4 style="margin-top:0">Usage (CLI)</h4>
        <p class="muted">
          Example run (commands you type at prompts):
        </p>
        <pre><code>
> python attendance.py
==== Attendance Management System ====
1. Add Attendance
2. View Attendance
3. Search Attendance by Name
4. Exit
Enter your choice: 1
Enter student name: Alice
Enter status (Present/Absent): Present
✅ Attendance marked for Alice on 2025-11-07 as Present
        </code></pre>
        <div style="margin-top:10px">
          <button class="btn" onclick="document.getElementById('demo').scrollIntoView()">Open demo below</button>
        </div>
      </aside>
    </div>

    <!-- Optional live demo: client-side version using localStorage -->
    <section id="demo" class="card">
      <h3>Live demo (client-side)</h3>
      <p class="muted">This small browser demo mimics the script using <code>localStorage</code>. It’s for demonstration only (does not modify CSV).</p>

      <div style="display:grid; grid-template-columns:1fr 420px; gap:16px;">
        <div>
          <form id="attendanceForm">
            <label class="muted">Student name</label>
            <input id="name" required placeholder="Enter name" />
            <label class="muted">Status</label>
            <select id="status">
              <option>Present</option>
              <option>Absent</option>
            </select>
            <div style="display:flex;gap:8px;margin-top:8px">
              <button type="submit" class="btn">Add</button>
              <button type="button" id="clearBtn" class="btn">Clear All</button>
            </div>
          </form>

          <div style="margin-top:14px">
            <label class="muted">Search by name</label>
            <input id="search" placeholder="Search..." />
          </div>

          <div style="margin-top:14px">
            <h4 style="margin:0 0 8px 0">Records</h4>
            <div id="recordsWrap" style="max-height:320px;overflow:auto;"></div>
          </div>
        </div>

        <div style="padding:10px;background:rgba(255,255,255,0.01);border-radius:8px;">
          <strong class="muted">How demo maps to script</strong>
          <ul class="muted">
            <li>Add attendance → appends a record with today's date</li>
            <li>View attendance → lists stored records</li>
            <li>Search → filters records by name</li>
            <li>Storage → HTML demo uses <code>localStorage</code>, Python uses <code>attendance.csv</code></li>
          </ul>
        </div>
      </div>

      <div style="margin-top:12px">
        <table id="recordsTable" aria-label="Attendance records">
          <thead>
            <tr><th>Name</th><th>Date</th><th>Status</th></tr>
          </thead>
          <tbody id="recordsBody"></tbody>
        </table>
      </div>
    </section>

    <footer>
      <div class="muted">Add this HTML snippet to your README or use it as a standalone demo page.</div>
    </footer>
  </div>

  <script>
    // Client-side demo logic (localStorage)
    const STORAGE_KEY = 'attendance_records_v1';

    function loadRecords(){
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        return raw ? JSON.parse(raw) : [];
      } catch(e){ return []; }
    }
    function saveRecords(list){
      localStorage.setItem(STORAGE_KEY, JSON.stringify(list));
    }
    function todayDate(){
      const d = new Date();
      return d.toISOString().slice(0,10);
    }

    function render(records){
      const tbody = document.getElementById('recordsBody');
      tbody.innerHTML = '';
      for(const r of records){
        const tr = document.createElement('tr');
        const tdName = document.createElement('td');
        tdName.textContent = r.name;
        const tdDate = document.createElement('td'); tdDate.textContent = r.date;
        const tdStatus = document.createElement('td'); tdStatus.textContent = r.status;
        tdStatus.className = r.status === 'Present' ? 'status-present' : 'status-absent';
        tr.append(tdName, tdDate, tdStatus);
        tbody.appendChild(tr);
      }
    }

    (function initDemo(){
      const form = document.getElementById('attendanceForm');
      const recordsWrap = document.getElementById('recordsWrap');
      const search = document.getElementById('search');
      const clearBtn = document.getElementById('clearBtn');

      let records = loadRecords();
      render(records);

      form.addEventListener('submit', (e)=>{
        e.preventDefault();
        const name = document.getElementById('name').value.trim();
        const status = document.getElementById('status').value;
        if(!name) return alert('Enter a name');
        const record = { name, date: todayDate(), status };
        records.push(record);
        saveRecords(records);
        render(records);
        form.reset();
      });

      search.addEventListener('input', ()=>{
        const q = search.value.trim().toLowerCase();
        if(!q) render(records);
        else render(records.filter(r=>r.name.toLowerCase().includes(q)));
      });

      clearBtn.addEventListener('click', ()=>{
        if(confirm('Clear all demo records?')) {
          records = [];
          saveRecords(records);
          render(records);
        }
      });
    })();
  </script>
</body>
</html>
