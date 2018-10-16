Добрый день, Денис!

Денис, получилось сложнее, чем я думал.
Итак, кратко:
1) Мой личный комп еще не прибыл, появится недели через полторы, поэтому все делаю на рабочем.
2) На рабочем не могу пока устанавливать SQLLite, могу только с MySQL работать, он установлен и давно. Там что на 1.5 недели обречен в проектах на Java работать с MySQL в качетве б.д.
3) МуSQL установлен, скрипты для таблицы user в этой же директории я прилагаю, в MySQL Workbench могу посмотреть, что записи юзеров есть.
4) В коде в Client не добавлял окно для аторизации, а ввеле счетчик случайных чисел - 0 или 1. 
Если 0, то успешно авторизовался юзер Bob, если 1, то Nikolay.
Показатель того, что с базами данных приложение умеет работать было то, что по логину и паролю из б.д. читалось имя авторизовавшегося юзера и выводилось в
Title окна чата. Код посмотреть можно тут - MultithreadChat\Client\src\client\Client.java
5) И вот тут я споткнулся на 4-5 часов, что даже немного меня позлило. Поясню.
Ранее драйвер работы с MySQL для Java не устанавливал, думал, что это проще пареной репы. Ан, нет.
В начале потребовалось добавлять из Maven Repository добавлять mysql-connector-java-version-bin.jar как mysql:mysql-connector-java:5.1.40.jar 
Воспользовался вот этой инструкцией - https://stackoverflow.com/questions/30651830/use-jdbc-mysql-connector-in-intellij-idea 
Он добавился и распознавался.

Но коннекции к моей базе данных на MySQL упорно не происходило.
Совершенно не понимал почему.
В Client.java на строчке 
	Connection conn=DriverManager.getConnection("jdbc:mysql://localhost:3389/world","root","DeltaDental!");
выбрасывает в исключение, что не может установить коннекцию.
При этом все, что тут есть - 1.порт, 2)имя базы данных, 3)логин, 4) пароль

При этом в коде в другом проекте на PHP данная строка успешно устанавливает подсоединение к MySQL базе данных -
$connection = mysqli_connect("localhost","root","DeltaDental!","world");

То есть логически 2)имя базы данных, 3)логин, 4) пароль ВЕРНЫ!
Остается порт. 

Попробовал без номера порта как "jdbc:mysql://localhost/world", тогда ошибка - 
Sun Oct 14 00:03:56 PDT 2018 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Could not create connection to database server.

На форуме https://stackoverflow.com/questions/34189756/warning-about-ssl-connection-when-connecting-to-mysql-database предлагаются варианты.

В MySQL выполнил команду show variables like 'port';
выдала 3306

А на Connection conn=DriverManager.getConnection( "jdbc:mysql://localhost:3306/world?useSSL=false","root","DeltaDental!");
результат - исключение "com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Could not create connection to database server."

Честно говоря, даже не знаю, что еще предположить и попробовать, чтобы коннекцию к моей MySQL базе данных world открыть.
Провозился уже больше 5 часов в совокупности, надо бы открыть еще и для других проеков и д.з.

Есть идеи или предложения, Денис, в чем причина или что попробовать?

Спасибо,
Александр