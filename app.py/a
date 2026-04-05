from flask import Flask, render_template, request

app = Flask(__name__)

def calculate_score(data):
    bw = float(data["bodyweight"])
    gender = data["gender"]
    age = int(data["age"])

    # Lifts
    bench = float(data["bench"])
    squat = float(data["squat"])
    deadlift = float(data["deadlift"])
    overhead = float(data["overhead"])

    # Ratios
    bench_r = bench / bw
    squat_r = squat / bw
    deadlift_r = deadlift / bw
    overhead_r = overhead / bw

    # Base score
    score = (bench_r * 1.2 +
             squat_r * 1.5 +
             deadlift_r * 1.7 +
             overhead_r * 1.0) * 100

    # Gender adjustment
    if gender == "female":
        score *= 0.9

    # Age adjustment
    if age < 18:
        score *= 0.85
    elif age > 40:
        score *= 0.9

    return round(score, 1)


def get_rank(score):
    if score < 200:
        return "Beginner"
    elif score < 300:
        return "Novice"
    elif score < 400:
        return "Intermediate"
    elif score < 500:
        return "Advanced"
    else:
        return "Elite"


@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        data = request.form
        score = calculate_score(data)
        rank = get_rank(score)

        return render_template("index.html", score=score, rank=rank)

    return render_template("index.html", score=None, rank=None)


if __name__ == "__main__":
    app.run(debug=True)
