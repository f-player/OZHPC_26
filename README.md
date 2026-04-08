# Задание-4
Малышева ЕО ИУ5-63б

Описание схемы
Блок ПРИБОР: моделирует цикл поломки и ремонта прибора. Прибор ломается (Tслом), переходит в состояние «сломан», ждёт свободного мастера, после ремонта возвращается в рабочее состояние.
Блок МАСТЕР: моделирует занятость мастера. Ромб проверяет состояние «мастер занят/свободен»; при занятости выполняется ремонт (Tрем), при свободе — ожидание.
Параметры: сост (состояние), режим, мастер, Трем — связывают оба блока в единую ОПС.

Markdown с Mermaid
```mermaid
flowchart TD

%% ========== ПОДГРАФ: ПРИБОР ==========
subgraph Pribor ["🔧 Блок ПРИБОР"]
    direction TB
    p_title[<em>ПРИБОР</em>]
    
    p_h1(["ВРЕМЯ=Tслом"]) ==> p_h2["сост:=<br>сломан"]
    p_h2 ==> p_h3(["режим=<br>работа"])
    p_h3 ==> p_h4(["мастер<br>=своб"])
    p_h4 ==> p_h5["мастер:=занят<br>Tрем:=func(x)"]
    p_h5 ==> p_h6(["ВРЕМЯ=Трем"])
    p_h6 ==> p_h7["сост:=рабочий<br>мастер:=своб<br>Tслом:=func(x)"]
    p_h7 ==> p_h1
end

%% ========== ПОДГРАФ: МАСТЕР ==========
subgraph Master ["👨‍🔧 Блок МАСТЕР"]
    direction TB
    m_title[<em>МАСТЕР</em>]
    
    %% Входы
    m_in1[/"1"/]
    m_in2[/"Траб = 9ч"/]
    
    %% Основные блоки
    m_h1["режим:=<br>отдых"]
    m_h2(["ВРЕМЯ=Траб"])
    m_h3["режим:=работа<br>Tотд:=func_..."]
    m_h4(["ВРЕМЯ=Тотд"])
    m_h5["Траб:=func_..."]
    m_h15{"мастер<br>=..."}
    m_h16["Трем:=Трем+<br>Траб-ВРЕМЯ"]
    
    %% Связи внутри Мастера
    m_in1 -.-> m_h1
    m_in2 -.-> m_h2
    m_h1 ==> m_h2
    m_h2 ==> m_h3
    m_h3 ==> m_h4
    m_h4 ==> m_h5
    m_h5 ==> m_h15
    m_h15 -- "...= занят" --> m_h16
    m_h16 ==> m_h1
    m_h15 -- "...= своб" --> m_h1
end

%% ========== ОБЩИЕ ПАРАМЕТРЫ (вне подграфов!) ==========
%% Параметры-круги
par1((режим))
par2((мастер))
par3((сост))
par4((Трем))

%% Связи Прибор ↔ Параметры
p_h2 -.-> par3
p_h7 -.-> par3
par1 -.-> p_h3
par2 -.-> p_h4
p_h5 -.-> par2
p_h7 -.-> par2
p_h5 -.-> par4
par4 -.-> p_h6

%% Связи Мастер ↔ Параметры
par2 -.-> m_h15
m_h1 -.-> par1
m_h3 -.-> par1
m_h16 -.-> par4

%% Инициаторы
Ini{{"I::"}} -.-> p_h1
HTf{{"I::Tслом=100"}} -.-> p_h1

%% ========== СТИЛИ ==========
classDef cond fill:#bee,stroke:#aaa,stroke-width:1px;
classDef state fill:#9e8,stroke:#333,stroke-width:1px;
classDef navig fill:#eda,stroke:#333,stroke-width:1px;
classDef inputPurple fill:#e6e6fa,stroke:#800080,stroke-width:1px;
classDef paramRejim fill:#ffc0cb,stroke:#333,stroke-width:2px;
classDef paramMaster fill:#e30b5d,stroke:#333,stroke-width:2px;
classDef paramSost fill:#aee,stroke:#333,stroke-width:2px;
classDef paramTrem fill:#cccccc,stroke:#555,stroke-width:2px;

%% Применение стилей к узлам Прибора
class p_h5,p_h2,p_h7 state;
class p_h1,p_h3,p_h4,p_h6 cond;
style p_title fill:yellow,stroke:red;

%% Применение стилей к узлам Мастера
class m_h1,m_h3,m_h5,m_h16 state;
class m_h2,m_h4 cond;
class m_h15 navig;
class m_in1,m_in2 inputPurple;
style m_title fill:yellow,stroke:red;

%% Применение стилей к параметрам
class par1 paramRejim;
class par2 paramMaster;
class par3 paramSost;
class par4 paramTrem;

%% ========== ГИПЕРССЫЛКИ ==========
click par2 href "https://iu5.bmstu.ru" "переход для Мастера" _blank
click par4 href "https://iu5.bmstu.ru" "переход для Трем" _blank
click par1 href "https://iu5.bmstu.ru" "переход для режима" _blank
click par3 href "https://iu5.bmstu.ru" "переход для состояния" _blank


