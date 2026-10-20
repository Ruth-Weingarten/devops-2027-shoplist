#מיבאים את הimage בסיסי של פייתון
FROM python:3.12-slim
#ממקמים בתיקיית העבודה
WORKDIR /app
#מעתיקים תלויות לספריות שצריך להתקין
COPY requirements.txt
#מריצים את ההתקנה של הספריות
RUN pip install --no-cache-dir requirements.txt
#מעתיקים את הקוד
COPY app.py .
#שומרים משתנה סביבה
ENV GREETING=Hello
#מאזינים על איזה פורט האפליקציה רץ
EXPOSE 8000
#הפקודה שרצה כשהקונטיינר עולה
cmd ["python","app.py"]
