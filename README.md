Основы работы с Unity
Отчет по лабораторной работе #3 выполнил(а):
- Кулаков Иван Александрович
- РИ300003

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * |   60 |
| Задание 2 | * |   20 |
| Задание 3 | * |   20 |

## Цель работы
Интеграция интерфейса пользователя в разрабатываемое интерактивное приложение.
## Задание 1
### Используя видео-материалы практических работ 1-5 повторить реализацию игровых механик:
### - 1 Практическая работа «Реализация механизма ловли объектов».
### – 2 Практическая работа «Реализация графического интерфейса с добавлением счетчика очков».
Ход работы:
![image](https://github.com/Snoubort/Game-Servases-Lab3/blob/main/GameWorkShort.gif)
- Создаём скрипт EnergyShield
- В скрипте щита добавляем управление щитом при помощи движения мыши
- Создадим холст, в верхний правый угол добавим счётчик
- При соприкосновении щита с объектом типа Dragon Egg уничтожаем яйцо, добавляем 1 к счёту
- Пример работы игры смотри в гифке в начале задания 1


        public class EnergyShield : MonoBehaviour
        {
            public TextMeshProUGUI scoreGT;

            void Start() {
                GameObject scoreGO = GameObject.Find("Score");
                scoreGT = scoreGO.GetComponent<TextMeshProUGUI>();
                scoreGT.text = "0";
            }

            void Update() {
                Vector3 mousePos2D = Input.mousePosition;
                mousePos2D.z = -Camera.main.transform.position.z;
                Vector3 mousePos3D = Camera.main.ScreenToWorldPoint(mousePos2D);
                Vector3 pos = this.transform.position;
                pos.x = mousePos3D.x;
                this.transform.position = pos;
            }

            private void OnCollisionEnter(Collision coll) {
                GameObject Collided = coll.gameObject;
                if(Collided.tag == "Dragon Egg"){
                    Destroy(Collided);
                }
                int score = int.Parse(scoreGT.text);
                score +=1;
                scoreGT.text = score.ToString();
            }
        }


## Задание 2
### – 3 Практическая работа «Уменьшение жизни. Добавление текстур».
### – 4 Практическая работа «Структурирование исходных файлов в папке».
- В скрипте DragonPicker создаём список щитов
- В цикл создания щитов на старте добавляем строчку с добавлением щита в ранее созданный список
- В этом же скрипте в функцию DragonEggDestroyd добавляем код, который при падении яйца мимо щита уменьшает размер массива щитов на 1, а так же удаляет последний щит(поскольку добавлялись они по мере создания, то он же и самый большой)
- Здесь же добавляем условие рестарта игры при 0 щитов
- Демонстрация в гифке в первом задании


        public class DragonPicker : MonoBehaviour
        {
            public GameObject energyShieldPrefab;
            public int numEnergyShield = 3;
            public float energyShieldBottomY = -6f;
            public float energyShieldRadius = 1.5f;

            public List<GameObject> shieldList;

            void Start()
            {
                shieldList = new List<GameObject>();
                for (int i = 1; i<= numEnergyShield; i++){
                    GameObject tShieldGo = Instantiate<GameObject>(energyShieldPrefab);
                    tShieldGo.transform.position = new Vector3(0, energyShieldBottomY, 0);
                    tShieldGo.transform.localScale = new Vector3(1*i, 1*i, 1*i);
                    shieldList.Add(tShieldGo);
                }
            }

            // Update is called once per frame
            void Update()
            {

            }

            public void DragonEggDestroyd(){
                GameObject[] tDragonEggArray = GameObject.FindGameObjectsWithTag("Dragon Egg");
                foreach (GameObject tGO in tDragonEggArray){
                    Destroy(tGO);
                }
                int shieldIndex = shieldList.Count -1;
                GameObject tShieldGo = shieldList[shieldIndex];
                shieldList.RemoveAt(shieldIndex);
                Destroy(tShieldGo);

                if (shieldList.Count == 0){
                    SceneManager.LoadScene("_0Scene");
                }
            }
        }
        
  
- Удаляем лишние папки, создаём папки с говорящими названиями, переносим задействованный в проекте контент в них. 
- Остальное - удаляем     

![Скрин интеграция](https://github.com/Snoubort/Game-Servases-Lab3/blob/main/MatForReadMe/ClearFolder.PNG?raw=true "Интеграция")

## Задание 3
### – 5 Практическая работа «Интеграция игровых сервисов в готовое приложение».
- Делаем сборку приложения, выставляя параметры(предварительно установив модуль юнити для WebGL)
![Скрин интеграция](https://github.com/Snoubort/Game-Servases-Lab3/blob/main/MatForReadMe/BasicBuild.PNG?raw=true "Интеграция")
- Скачиваем плагин и перетаскиваем файл в мэнеджэр пути Unity, импортируем
- Добавляем на сцену объект YandexGame
![Скрин интеграция](https://github.com/Snoubort/Game-Servases-Lab3/blob/main/MatForReadMe/YGObj.PNG?raw=true "Интеграция")
- Устанавливаем дополнительные настройки, делаем билд для яндекс игр
![Скрин интеграция](https://github.com/Snoubort/Game-Servases-Lab3/blob/main/MatForReadMe/YGBuild.PNG?raw=true "Интеграция")
- Билд яндекса упаковываем в zip, после чего загружаем через панель разработчика яндекс и ждём окончания проверки(не забываем перезагружать страницу, ибо иначе состояние проверки не обновится)
![Скрин интеграция](https://github.com/Snoubort/Game-Servases-Lab3/blob/main/MatForReadMe/YGConsole.PNG?raw=true "Интеграция")
- Проходим по ссылке, ждём загрузки, играем
![Скрин интеграция](https://github.com/Snoubort/Game-Servases-Lab3/blob/main/MatForReadMe/YGPlay.PNG?raw=true "Интеграция")
