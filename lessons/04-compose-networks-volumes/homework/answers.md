<div dir="rtl">

# שיעור 4 - תשובות

## 1. ל־`backend` אין `ports`. איך `stats` מגיע אליו? ואיך הדפדפן מגיע אליו?

`stats` ניגש ל־`backend` דרך הרשת הפנימית ש־Docker Compose יצר, בפנייה ישירה לפי השם: `http://backend:5000`.

הדפדפן לא ניגש ל־`backend` ישירות. הוא נכנס דרך הדלת החיצונית היחידה, פורט `8080`, שמחוברת לפורט `80` של ה־`frontend`. שם Nginx מעביר כל בקשה שמתחילה ב־`/api/` אל ה־`backend`.

## 2. עושים `docker compose down` ואחר כך `up`. המוצרים נשארו? ומה אם עושים `down -v`? הסבירו לפי סוג ה־volume.

בהתחלה המוצרים נשארו, כי הם שמורים ב־named volume בשם `postgres-data`, שנמצא מחוץ לקופסה. `down` מוחק רק את הקופסאות, לא את ה־volumes.

כשעושים `down -v`, מוחקים גם את ה־volumes, כלומר גם את העותק ה"קבוע", ולכן המוצרים לא נשארים.

## 3. השורה `../sql/init.sql:/docker-entrypoint-initdb.d/init.sql`: איזה סוג volume זה, ואיך יודעים? מה יקרה אם תשנו את `init.sql` אחרי שהמסד כבר נוצר?


## 4. מה שם הרשת ש־Compose יצר, ומה שם ה־volume? מאיפה הגיעו השמות?

שם הרשת: `docker_default`. שם ה־volume: `docker_postgres-data`.

השמות הגיעו מהתיקייה `docker`, שבה נמצא קובץ ה־`docker-compose.yml`. ‏Compose קורא לפרויקט על שם התיקייה, ומוסיף את השם הזה כקידומת לכל מה שהוא יוצר. `default` זו הרשת שהוא יוצר כשלא מגדירים רשת בעצמנו, ו־`postgres-data` זה השם שנתנו ל־volume בקובץ.

## 5. ב־`Dockerfile.multistage`: מה יש ב־image הסופי ומה אין? למה אין `CMD`? ולמה ל־`stats` לא צריך multi-stage?

ב־image הסופי יש רק Nginx, את האתר המוכן מתיקיית `dist` ואת `nginx.conf`. אין בו Node, אין `build.js` ואין את קבצי המקור, הם נשארו בשלב הבנייה.

אין `CMD` כי שלב 2 מתחיל מ־`nginx:1.30-alpine`, וה־image הזה כבר כולל `CMD` שמפעיל את Nginx. הוא עובר בירושה.

ל־`stats` לא צריך multi-stage כי אין לו שלב בנייה: הוא צריך את Node כדי לרוץ (`node server.js`), אז אין כלים שאפשר להשאיר מאחור.

## 6. שלושת הגדלים מ־ד8 (`single`, `multi`, `kitchen`). למה `multi` כמעט בגודל של `single`?

`single`: 92.7MB, `multi`: 92.7MB, `kitchen`: 244MB.

`multi` כמעט בגודל של `single` כי בשניהם יש רק את `nginx:1.30-alpine` ועוד כמה קבצי אתר קטנים. באף אחד מהם אין Node, שהוא החלק הכבד. `kitchen` גדול בהרבה כי Node נמצא בו.

## 7. ב־`nginx.conf`: בקשה ל־`/api/stats` מתאימה גם ל־`location /api/`. למה היא מגיעה ל־`stats`? ומה קורה לבקשה `/api/products` בגלל הלוכסן בסוף `http://backend:5000/`?

שלוש הוראות מתאימות ל־`/api/stats`: `/`, `/api/` ו־`/api/stats`. ‏Nginx בוחר את ההתאמה הארוכה ביותר, `/api/stats`, ולכן הבקשה מגיעה ל־stats.

בגלל הלוכסן בסוף `http://backend:5000/`, ‏Nginx מוריד את `/api/` לפני שהוא מעביר את הבקשה, כי ה־backend לא מכיר את `/api`. כך `/api/products` מגיע ל־backend בתור `/products`.

</div>
