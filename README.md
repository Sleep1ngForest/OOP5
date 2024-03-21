Задание.

Реализуйте удаление пользователей.

Подумать, где должен находиться метод createUser из UserView и если получится, вынести его в нужный слой.

Вынести логику dao в нужный слой (репозиторий), а от слоя dao избавится физически. ВАЖНО: ознакомиться с статьей: [https://habr.com/ru/articles/263033](https://javarush.com/groups/posts/2536-chastjh-7-znakomstvo-s-patternom-mvc-model-view-controller)/

На выбор (не обязательно):

1. проанализировать проект и реализовать нереализованные методы.

2. дописать код для оставшихся команд в Commands (можно реализовать сохранение списка USer)
ИЛИ ВНЕСИТЕ СВОИ ИЗМЕНЕНИЯ В ПРОЕКТ, КОТОРЫЕ КАЖУТЬСЯ ЛОГИЧНЫМИ ВАМ.

РЕАЛИЗАЦИЯ

1. УДАЛЕНИЕ ПОЛЬЗОВАТЕЛЕЙ
2. 
3. В UserRepository(repository) переопределяем метод public boolean delete(Long id)
4. 
5. В UserController(controller) прописываем метод public boolean deleteUser(Long userid)
6. 
7. В UserView(view) добавляем стр 44-47 команда DELETE

8. 
2. ПОДУМАТЬ, ГДЕ ДОЛЖЕН НАХОДИТЬСЯ МЕТОД createUser ИЗ UserView И ВЫНЕСТИ ЕГО В НУЖНЫЙ СЛОЙ
3. 
     Перенесла в UserController(controller). Сделала метод из private в public и добавила метод private String scan(String message)
   
     В UserView необходимо внести изменения в строку 28 и 50: userController.createUser()
   
3.Вынести логику dao в нужный слой (репозиторий), а от слоя dao избавится физически.

Слой dao логичнее всего перенести в repository

1.Из интерфейса Operation необходимо все команды перенести в интерфейс GBRepository

2.Далее перенести код из FileOperation в UserRepository

Кусок кода в UserRepository, который был до изменений:

public class UserRepository implements GBRepository {
    private final UserMapper mapper;
    private final FileOperation operation;
    public UserRepository(FileOperation operation) {
        this.mapper = new UserMapper();
        this.operation = operation;
    }
    
    После изменений код бужет выглядеть так:
    
    public class UserRepository implements GBRepository {
    private final UserMapper mapper;
    private final String fileName;
    public UserRepository(String fileName) {
        this.fileName = fileName;
        this.mapper = new UserMapper();
        try (FileWriter writer = new FileWriter(fileName, true)) {
            writer.flush();
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
    }
    
Код из FileOperation в части методов readAll и saveAll переносится в UserRepository без изменений копированием

3. В Main изменятся строки:
4. 
FileOperation fileOperation = new FileOperation(DB_PATH);
GBRepository repository = new UserRepository(fileOperation);

В измененном виде останется 

GBRepository repository = new UserRepository(DB_PATH);

6. во всем проекте убиваем import notebook.model.dao.impl.FileOperation;
7. 
8. удаляем слой dao
9. 
10. тестируем Main на работоспособность - у меня все получилось!
11. 
