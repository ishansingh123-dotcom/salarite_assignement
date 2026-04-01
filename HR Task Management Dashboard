
import gradio as gr
import pandas as pd
import random
import datetime

# ---------------------------------------------------------
# GENERATE DASHBOARD DATA
# ---------------------------------------------------------

def generate_dashboard():

    employer_name = "NextGen Talent Solutions Pvt Ltd"

    employees = [
        "Rohan Sharma",
        "Ishan Singh",
        "Mohan Verma",
        "Rohit Kumar",
        "Vandana Gupta"
    ]

    departments = [
        "Recruitment",
        "HR Operations",
        "Verification",
        "Talent Acquisition",
        "HR Support"
    ]

    tasks = [
        "Screen Resume",
        "Conduct Interview",
        "Verify Documents",
        "Update Candidate Status",
        "Send Offer Letter"
    ]

    priority = ["High", "Medium", "Low", "High", "Medium"]

    status = [
        "Assigned",
        "In Progress",
        "Completed",
        "In Progress",
        "Assigned"
    ]

    progress = [random.randint(30, 100) for _ in range(5)]

    today = datetime.date.today()

    def schedule_interview():
        base = datetime.datetime.now()
        interview_time = base + datetime.timedelta(
            hours=random.randint(1, 48)
        )
        return interview_time.strftime("%Y-%m-%d %H:%M")

    interview_times = [schedule_interview() for _ in range(5)]

    df = pd.DataFrame({
        "Employer": [employer_name] * 5,
        "Virtual HR": employees,
        "Department": departments,
        "Assigned Task": tasks,
        "Priority": priority,
        "Status": status,
        "Progress (%)": progress,
        "Interview Time": interview_times,
        "Assigned Date": [today] * 5
    })

    df["Performance Score"] = [
        random.randint(70, 100)
        for _ in range(len(df))
    ]

    df["Last Updated"] = datetime.datetime.now().strftime(
        "%Y-%m-%d %H:%M:%S"
    )

    total = len(df)
    completed = len(df[df["Status"] == "Completed"])
    in_progress = len(df[df["Status"] == "In Progress"])
    pending = total - completed - in_progress

    return df, total, completed, in_progress, pending


# ---------------------------------------------------------
# BUTTON FUNCTIONS
# ---------------------------------------------------------

def refresh_dashboard():
    df, total, completed, in_progress, pending = generate_dashboard()
    return df, total, completed, in_progress, pending


def export_to_excel(df):
    file_name = "HR_Dashboard_Report.xlsx"
    df.to_excel(file_name, index=False)
    return "Excel file downloaded successfully"


def search_employee(name):
    df, _, _, _, _ = generate_dashboard()
    result = df[
        df["Virtual HR"].str.contains(
            name,
            case=False
        )
    ]
    return result


def add_employee(name):
    return f"Employee {name} added successfully"


def delete_employee(name):
    return f"Employee {name} deleted successfully"


def generate_report():
    df, total, completed, in_progress, pending = generate_dashboard()

    return (
        f"Total Tasks: {total}\n"
        f"Completed: {completed}\n"
        f"In Progress: {in_progress}\n"
        f"Pending: {pending}"
    )


def reset_dashboard():
    empty = pd.DataFrame()
    return empty, 0, 0, 0, 0


def schedule_new_interview():
    return "New interview scheduled successfully"


# ---------------------------------------------------------
# UI DESIGN
# ---------------------------------------------------------

custom_css = """
body {
    background-color: #eef3ff;
}
.title {
    text-align:center;
    font-size:36px;
    font-weight:bold;
    color:white;
    background: linear-gradient(90deg,#007bff,#6f42c1);
    padding:16px;
    border-radius:10px;
}
"""

with gr.Blocks(css=custom_css) as demo:

    gr.Markdown(
        "<div class='title'>SMART HR MANAGEMENT DASHBOARD</div>"
    )

    with gr.Row():
        refresh_btn = gr.Button("Refresh Dashboard")
        export_btn = gr.Button("Export to Excel")
        report_btn = gr.Button("Generate Report")
        schedule_btn = gr.Button("Schedule Interview")
        reset_btn = gr.Button("Reset Dashboard")

    name_input = gr.Textbox(
        label="Employee Name"
    )

    with gr.Row():
        search_btn = gr.Button("Search Employee")
        add_btn = gr.Button("Add Employee")
        delete_btn = gr.Button("Delete Employee")

    total_box = gr.Number(label="Total Tasks")
    completed_box = gr.Number(label="Completed")
    progress_box = gr.Number(label="In Progress")
    pending_box = gr.Number(label="Pending")

    table = gr.Dataframe()

    message = gr.Textbox(label="System Message")

    # BUTTON ACTIONS

    refresh_btn.click(
        refresh_dashboard,
        outputs=[
            table,
            total_box,
            completed_box,
            progress_box,
            pending_box
        ]
    )

    export_btn.click(
        export_to_excel,
        inputs=table,
        outputs=message
    )

    search_btn.click(
        search_employee,
        inputs=name_input,
        outputs=table
    )

    add_btn.click(
        add_employee,
        inputs=name_input,
        outputs=message
    )

    delete_btn.click(
        delete_employee,
        inputs=name_input,
        outputs=message
    )

    report_btn.click(
        generate_report,
        outputs=message
    )

    schedule_btn.click(
        schedule_new_interview,
        outputs=message
    )

    reset_btn.click(
        reset_dashboard,
        outputs=[
            table,
            total_box,
            completed_box,
            progress_box,
            pending_box
        ]
    )

# IMPORTANT FOR DEPLOYMENT

if __name__ == "__main__":
    demo.launch(server_name="0.0.0.0", server_port=7860)
