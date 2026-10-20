איזו פקודה תבנה מה-Dockerfile שלכן image בשם greeter עם התגית v1? (מהתיקייה הראשית של הריפו.)
#docker build -t greeter:v1 lessons/02-docker-basics/homework/greeter
איזו פקודה תריץ אותו ברקע, בשם greeter, כך שבדפדפן נגיע אליו דרך http://localhost:9000?
#docker run -d --name greeter -p 9000:8000 greeter:v1
מה צריך להוסיף לפקודה משאלה 2 כדי שהברכה תהיה Shalom במקום Hello?
#-e GREETINGS=halom
מה תחזיר הכתובת http://localhost:9000/hello/Rivka אחרי השינוי משאלה 3?
#{"message": "Shalom, Rivka!"}
שיניתן שורה ב-app.py ובניתן שוב. אילו שלבים ב-build יסומנו CACHED, ואילו ירוצו מחדש? למה?
#השלבים FROM, WORKDIR, COPY requirements.txt ו-RUN pip install יסומנו CACHED, 
#כי הם לא השתנו. השלב COPY app.py וכל השלבים שאחריו (ENV, EXPOSE, CMD)
#ייבנו מחדש, כי תוכן app.py השתנה, ושינוי בשכבה שובר את ה-cache של כל השכבות שאחריה.
