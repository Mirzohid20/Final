# Final
This is a project that creates a simple application showing the courses that can be taken for the next semester. In the beginning, we created in AWS database and instance. We linked them based on the Linux operating system, we enter the given commands.





Sorce code HTML:
<!DOCTYPE html>
<html>
<head>
    <title>Mini Project of Mirzohid</title> 
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: top;
            justify-content: top;
            margin: 0;
            height: 100vh;
            background-color: #5a5a5a;
        }

        h1 {
            color: #333;
        }

        form {
            margin-bottom: 20px;
            text-align: top;
        }

        label {
            font-size: 16px;
            font-weight: bold;
        }

        select {
            padding: 5px;
            font-size: 14px;
            margin-left: 10px;
        }

        button {
            background-color: #a73d28;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            border-radius: 5px;
            margin-left: 10px;
        }

        button:hover {
            background-color: #882f21;
        }

        table {
            border-collapse: collapse;
            width: 60%;
            margin-top: 20px;
            background-color: white;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }

        th {
            background-color: #f2f2f2;
            font-size: 16px;
        }

        td {
            font-size: 14px;
        }
    </style>
</head>
<body>
    <h1>University Timetable</h1>
    <form method="POST">
        <label for="level">Select Level:</label>
        <select name="level" id="level">
            <option value="Undergraduate">Undergraduate</option>
            <option value="Graduate">Graduate</option>
        </select>
        <button type="submit">Submit</button>
    </form>
    {% if courses %}
    <table>
        <tr>
            <th>Course Name</th>
            <th>Day</th>
            <th>Time</th>
        </tr>
        {% for course in courses %}
        <tr>
            <td>{{ course[0] }}</td>
            <td>{{ course[1] }}</td>
            <td>{{ course[2] }}</td>
        </tr>
        {% endfor %}
    </table>
    {% endif %}
</body>
</html>                                                                                                                                           


Sorse code APP.PY:
from flask import Flask, render_template, request
import psycopg2

app = Flask(__name__)

# PostgreSQL connection
conn = psycopg2.connect(
    host="<RDS_Endpoint>",
    database="<postgres>",
    user="<postgres>",
    password="<postgres>"
)

@app.route("/", methods=["GET", "POST"])
def index():
    courses = []
    if request.method == "POST":
        level = request.form["level"]
        with conn.cursor() as cur:
            cur.execute("SELECT course_name, day, time FROM timetable WHERE level = %s", (level,))
            courses = cur.fetchall()
    return render_template("index.html", courses=courses)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8000, debug=True)


