
Базите данни за този проект се налага да бъдат добре структурирани, 
разбираеми и "flexible" за целите ни. На този етап, спрямо каквото сме
преценили за нужно, имаме базата данни accessment_db.sql в папката.
С цел да бъде разбираема, тук тя ще бъде по-подробно описана.

----------------------------------------------------------------------------

Като за начало ще представим кои са основните ни обекти.

Базата ни данни съдържа:
- Студенти
- Учители
- Админи
- Акаунти в платформата
- Курсове
- Quiz-ове
- Оценки на студентите
- Календар
- спомагателни таблици за обектите

----------------------------------------------------------------------------

Преди да бъде документирано коя таблица какви атрибути има, с цел улеснение,
първо ще обясня кой обект какви правомощия и функционалности има.

Студенти: те участват на дадени quiz-ове с цел изкарване на оценка; след
всеки quiz те изкарват определен брой точки, който чрез формула определя
каква оценка е изкарал и тази оценка се запазва за него

Учители: те имат функцията да създават quiz-ове за специалностите, които
преподават и да ги изпращат до студентите; на този етап това е единствената
им функционалност, понеже сегашния release на Accessify използва само
въпроси със затворен отговор (А,Б,В и Г); при доразвитие ако quiz-овете
имат и свободни отговори - тогава учителите ще имат и функцията да ги 
проверяват

Админи: те създават акаунтите в платформата и имат функцията на отговорници
ако в системата възникне проблем

Акаунти: те съдържат информацията като username, парола, кой каква роля
има в Accessify и т.н.

Курсове: те съдържат информация за всичките налични факултети, курсове и 
специалности; всеки студент е записан в една специалност, а учителите могат
да преподават по много такива

Quiz-ове: това са въпросници, които нямат фиксиран брой въпроси, но на този
етап имат фиксиран брой възможни отговори (А,Б,В и Г); quiz-овете се
създават от един учител и могат да бъдат изпратени до различни специалности,
като не е задължително да бъдат от един и същи факултет

Оценки: това е таблица, която в себе си съдържа информация за всички
студенти какви оценки имат от quiz-овете, структурирано така че студентите
да знаят за всеки един преминат quiz каква им е оценката

Календар: това цели да попълни календар за това всеки студент на какъв
quiz трябва да се яви и от кога до кога му е позволено; с цел повече
информативност и адаптация към front-end - има и добавени атрибути за това
в какъв цвят да се оцвети дадения quiz в календара и кой е учителя, който
е създал теста (може да бъде полезно за студентите)

Спомагателни таблици: те са за случаите, когато даден relation между обекти
е M:M или имаме нужда от спефичен вид справки; по надолу се разглежда всяка
таблица каква информация цели да предостави и защо

----------------------------------------------------------------------------

Идеята и спецификите зад всяка създадена таблица са както следва:

students: типича информация за студентите; като външни ключове има 
account_id, за това кой е предвидения профил за него, и course_id, който
да реферира към таблицата със специалностите

teachers: отново типична информация; външен ключ е account_id със същата
функционалност като при студентите

admins: на този етап съдържа същата информация като учителите

accounts: съхранява никнейм, парола, роля (A, T или S <-> за Админ, Учител
или Студент), информация дали сега този акаунт е на лиция и ако не - кога
е бил последно на линия

courses: съдържа информация за всичките специалности; цели се идентификация

quizes: съдържа като информация името на всеки quiz и това, кой учител го
е създал; в тази таблица се цели идентифициране на всеки quiz, тъй като при
другите таблици, свързани с quiz-ове, се налага често да използваме дадено
id на quiz

quiz_questions: съхранява се информация за въпросите в дадените quiz-ове;
какъв текст се визуализира, какъв е правилния отговор (това ще е за функция
през front-end) и колко точки носи дадения въпрос

quizes_for_course: съдържа информация кой quiz за коя специалност е 
предвиден; както стана ясно вече - даден quiz може да бъде изпратен до
много специалности; съхранява се и информация от кога до кога е предвиден
даден quiz

quizes_info: цели да съдържа информацията нужна за календара на всеки 
студент, за когато му предстои quiz; intended_quiz е външен ключ към
id на quizes_for_course, тъй като всеки quiz създаден там е уникален,
понеже е предвиден за различни специалности по различно време

grades_of_students: пази информация за всичките студенти на кои quiz-ове на
коя дата каква оценка са изкарали; това е в нужда както за front-end, така
и за справки и отчети

quizes_attended: тази таблица пази информация за това кой студент на кои
quiz-ове е участвал; целта е да направи справка ако случайно е имало 
нередности или се налага извеждане на даден отчет или статистика; не е
важна за функционалността за Accessify на този етап

teaching_courses: тази таблица съдържа информация за това кой преподавател
на кои специалности преподава; важно е за справки, но не и за Accessify на
този етап



