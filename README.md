Airflow_March_2026

Installing Apache Airflow on an M1/M2/M3 Mac (Apple Silicon) can be a bit finicky because of the architecture differences, but using a virtual environment makes it much smoother. Since you are using zsh and Python 3.9.6, we can get this running directly in your terminal.

1. Prepare Your Environment
First, create a dedicated directory and a virtual environment. This prevents Airflow's dependencies from breaking your system Python.
Bash

mkdir airflow_home
cd airflow_home
python3 -m venv venv
source venv/bin/activate

2. Set the Airflow Home Path
Permanent Path (Recommended)
To make sure your M1 always knows where Airflow lives, add it to your .zshrc file.
3. Open your zsh configuration:Bashnano ~/.zshrc
4. Add this line at the bottom (replace /your/path/to/airflow with your actual folder path):Bashexport AIRFLOW_HOME=/Users/neerajvyas/airflow_projects
5. Save and Exit: Press Ctrl + O, then Enter, then Ctrl + X.
6. Apply the changes:Bashsource ~/.zshrc

========================================
#. Install Airflow (The "Constraints" Method)
Airflow is sensitive to library versions. To ensure a smooth install on M1 chips, use the official constraint files provided by Apache.
Bash

# Define the Airflow version
AIRFLOW_VERSION=2.8.1

# Get your Python version (3.9)
PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"

# Construct the constraint URL
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"

# Install Airflow
pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"


#. Initialize and Start
Once installed, you need to set up the backend database (SQLite by default) and create a user so you can log in.
Initialize Database
Bash

airflow db init
Create an Admin User
Replace password with something you'll remember.
Bash

airflow users create \
    --username admin \
    --firstname Neeraj \
    --lastname Vyas \
    --role Admin \
    --email admin@example.com \
    --password admin
Start the Services
You will need two terminal tabs open with your virtual environment activated (source venv/bin/activate):
* Tab 1 (The Scheduler):cd /Users/neerajvyas/learning/airflow/learn_airflow/airflow_home airflow scheduler
* Tab 2 (The Webserver):cd /Users/neerajvyas/learning/airflow/learn_airflow/airflow_home airflow webserver --port 8080


Every time you need to do it on each terminal - scheduler and webserver.
python3 -m venv venv
source venv/bin/activate
======================================="One-Liner" commands.
Since your Airflow files are located at /Users/neerajvyas/learning/airflow/learn_airflow/airflow_home/, you can use the absolute path to the Python executable inside your venv.

Terminal 1: The Scheduler
Copy and paste this entire block:
Bash

export AIRFLOW_HOME=/Users/neerajvyas/learning/airflow/learn_airflow/airflow_home && \
/Users/neerajvyas/learning/airflow/learn_airflow/airflow_home/venv/bin/airflow scheduler
Terminal 2: The Webserver
Copy and paste this entire block:
Bash

export AIRFLOW_HOME=/Users/neerajvyas/learning/airflow/learn_airflow/airflow_home && \
/Users/neerajvyas/learning/airflow/learn_airflow/airflow_home/venv/bin/airflow webserver --port 8080

The "Pro" Way (Zsh Aliases)
Since you are using zsh on your M1, you can make this even easier. You can create shortcuts so you only have to type start_scheduler or start_webserver.
1. Open your zsh config: nano ~/.zshrc
2. Add these lines at the bottom:Bashalias air_env='export AIRFLOW_HOME=/Users/neerajvyas/learning/airflow/learn_airflow/airflow_home'
3. alias start_scheduler='air_env && /Users/neerajvyas/learning/airflow/learn_airflow/airflow_home/venv/bin/airflow scheduler'
4. alias start_webserver='air_env && /Users/neerajvyas/learning/airflow/learn_airflow/airflow_home/venv/bin/airflow webserver --port 8080'
5. Save (Ctrl+O, Enter, Ctrl+X) and refresh: source ~/.zshrc
Now, you can just open any terminal and type start_scheduler or start_webserver and it will work perfectly.

Verification
Once both are running, your Airflow UI should show a green status for the scheduler.


