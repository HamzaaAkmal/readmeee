# Attendance Management System

Assalam-o-Alaikum! Ye ek simple Attendance Management System hai, jo aapko students ki attendance track karne mein help karega. Isko Streamlit framework use karke banaya gaya hai, jisse iska user interface (UI) bohot easy aur interactive hai.

## Project Ka Maqsad (Purpose)

Is project ka basic maqsad hai:
*   Students ka record rakhna (add, update, delete).
*   Students ki daily attendance mark karna.
*   Attendance records ko date ya student ke hisab se dekhna.
*   System ko admin login ke through secure karna.

Ye kisi bhi university student ke liye ek acha project hai jo viva ya presentation ke liye practical application banana chahta hai.

## Kya Tools/Libraries Use Hue Hain? (Tools Used)

Humne is project mein ye cheezein use ki hain:

1.  **Streamlit:**
    *   Ye ek Python framework hai jo web applications banane ke liye use hota hai. Iski madad se humne graphical user interface (GUI) banaya hai. Isko sikhna aur use karna bohot aasan hai. `pip install streamlit` se install hota hai.

2.  **Pandas:**
    *   Ye bhi Python ki ek bohot powerful library hai data analysis aur manipulation ke liye. Hum isko data ko table (DataFrame) ki shakal mein dikhane aur process karne ke liye use karte hain. `pip install pandas` se install hota hai.

3.  **Python Standard Libraries:**
    *   Jaise `datetime` (dates aur times ko handle karne ke liye) aur `os` (file aur folder operations ke liye). Ye pehle se hi Python mein hote hain.

## Project Ki Banawat (Project Structure Explained)

Hamare project mein kuch files aur ek folder hai, jinko main simple zubaan mein samjhata hoon:

```
.
├── data/
│   ├── attendance.txt
│   └── students.txt
├── auth.py
├── config.py
├── main.py
├── managers.py
├── models.py
└── system.py
```

*   **`data/` Folder:**
    *   **Kaam:** Ye folder hamara "storage room" hai. Jab hum koi student add karte hain ya attendance mark karte hain, to uska data isi folder mein text files ki shakal mein save hota hai. Database ki zaroorat nahi padti.
    *   **Files iske andar:**
        *   `students.txt`: Ismein saare registered students ki information hoti hai (jaise ID aur naam). Har line mein ek student ka record hota hai.
        *   `attendance.txt`: Ismein saare students ki attendance records hoti hain (jaise student ID, naam, date aur status - Present ya Absent).

*   **`auth.py` File:**
    *   **Kaam:** Ye file "system ke darbaan" ki tarah hai. Ismein `AuthenticationManager` naam ki class hai jo user (admin) ko login aur logout karne ka kaam dekhti hai. Ye check karti hai ke sahi username aur password daala gaya hai ya nahi. Saath hi, ye yaad rakhti hai ke kaun login hai (`st.session_state` use karke).
    *   **Basic Logic:** Login screen par username aur password manga jata hai. Agar wo `config.py` mein diye gaye admin credentials se match karte hain, to login successful hota hai aur app ke andar ka content dikhna shuru ho jata hai.

*   **`config.py` File:**
    *   **Kaam:** Ye file "system ki settings" hai. Ismein saari important values (jaise data files kahan hain, admin ka username aur password kya hai) ek jagah define ki jati hain. Agar aapko koi setting badalni hai (maslan admin ka password), to sirf is file mein change karna hoga.
    *   **Basic Logic:** Ismein variables hain jaise `ADMIN_USERNAME`, `ADMIN_PASSWORD`, `STUDENTS_TXT`, `ATTENDANCE_TXT`. Ye values doosri files use karti hain.

*   **`models.py` File:**
    *   **Kaam:** Ye file "data ke blueprints" banati hai. Ismein `Student` aur `AttendanceRecord` naam ki classes hain. Ye classes define karti hain ke ek student ki information kaise store hogi (ID aur naam) aur ek attendance record ki information kaise store hogi (student ID, naam, date, status). Ye data ko object ki shakal mein handle karne mein madad karti hain.
    *   **Basic Logic:** Har class mein `to_dict()` aur `from_dict()` jaise functions hain jo objects ko dictionary (aasan data format) mein badalte hain aur wapas objects mein convert karte hain. Ye data ko file mein save aur load karne ke liye zaroori hain. `property` aur `setter` se data ki validation bhi hoti hai.

*   **`managers.py` File:**
    *   **Kaam:** Ye file "data ke dekhbhaal karne wale" hain. Ismein `StudentManager` aur `AttendanceRecorder` naam ki classes hain.
        *   `StudentManager`: `students.txt` file ko handle karta hai. Students ko add, update aur delete karta hai.
        *   `AttendanceRecorder`: `attendance.txt` file ko handle karta hai. Attendance mark karta hai aur records ko delete/read karta hai.
    *   **Basic Logic:** Har manager apne specific data file se data read (`_load_data`) karta hai aur usmein changes karke wapas save (`_save_data`) karta hai. Ye `models.py` ki classes ko use karte hue objects ko handle karte hain.

*   **`system.py` File:**
    *   **Kaam:** Ye file "poore system ka master control" hai. Ismein `AttendanceSystem` naam ki class hai jo `StudentManager` aur `AttendanceRecorder` ko ek saath combine karti hai. Ye `main.py` ko saari high-level functionalities provide karti hai (maslan "add student" bolna, to ye andar `StudentManager` ko call kar deta hai).
    *   **Basic Logic:** Ye `main.py` aur `managers.py` ke darmiyan pul (bridge) ka kaam karti hai. Jab koi student delete hota hai, to ye `StudentManager` aur `AttendanceRecorder` dono ko bolti hai ke us student ka data har jagah se delete kar do (data consistency). Ye dashboard ke liye overall statistics bhi calculate karti hai.

*   **`main.py` File:**
    *   **Kaam:** Ye hamara "app ka chehra" hai. Ye Streamlit app ko run karta hai aur user ko jo kuch bhi screen par dikhta hai (buttons, forms, tables), wo sab is file mein banaya gaya hai. Ye `auth.py` aur `system.py` se functions call karke data display aur update karta hai.
    *   **Basic Logic:**
        1.  App start hota hai, login screen dikhta hai.
        2.  Login hone ke baad, left sidebar mein navigation options aate hain (Dashboard, Student Management, Mark Attendance, View Records).
        3.  Aap jo option choose karte hain, uske hisab se main screen par content change hota hai aur `system.py` ke functions ko call karke data fetch ya update hota hai.
        4.  `st.session_state` use hota hai taake jab aap koi action perform karen ya page reload ho, to login status aur doosra data safe rahe.

## Code Ka Basic Logic Step-by-Step

1.  **App Start:** Jab aap `streamlit run main.py` karte hain, to `main.py` start hota hai.
2.  **Initialization:** `main.py` mein `AuthenticationManager` aur `AttendanceSystem` ke objects banate hain. Ye objects `st.session_state` mein store ho jate hain taake poore app mein unko access kiya ja sake aur state maintain rahe.
3.  **Login Check:** Sabse pehle `auth_manager.is_logged_in()` check hota hai. Agar aap logged in nahi hain, to login form dikhta hai (username: `admin123`, password: `admin123` - ye `config.py` se aata hai).
4.  **Login Action:** Agar aap `Login` button press karte hain, to `auth_manager.login()` call hota hai. Sahi credentials par login successful ho jata hai aur page `st.experimental_rerun()` se refresh ho jata hai.
5.  **Navigation:** Login ke baad, sidebar mein Dashboard, Student Management, Mark Attendance, aur View Attendance Records ke options aate hain.
6.  **Student Management:**
    *   Students `add` karne ke liye, aap name input karte hain, `system_manager.add_student()` call hota hai, jo internally `StudentManager.add_student()` ko use karta hai.
    *   Students `update` ya `delete` karne ke liye, `system_manager.update_student()` ya `system_manager.delete_student()` call hote hain. Ye functions `StudentManager` aur `AttendanceRecorder` dono mein data update/delete karte hain taake consistency rahe.
    *   `get_all_students_df()` `StudentManager` se students ki list `pandas DataFrame` ki shakal mein laata hai aur `st.dataframe()` use karke dikha deta hai.
7.  **Mark Attendance:**
    *   Aap date select karte hain.
    *   Saare students ki list dikhti hai, aur har student ke saamne checkbox hota hai. Jo students pehle se us date par mark hue hain, unke checkboxes pre-filled hote hain.
    *   Aap checkboxes tick/untick karke `Submit Attendance` karte hain.
    *   `system_manager.mark_attendance()` call hota hai, jo `AttendanceRecorder.mark_attendance()` ko use karke attendance data ko `attendance.txt` mein save karta hai.
8.  **View Attendance Records:**
    *   Aap option choose karte hain (Overall, By Date, By Student).
    *   `system_manager` ke corresponding functions (jaise `get_overall_attendance_df()`, `get_attendance_by_date_df()`, `get_attendance_by_student_df()`) call hote hain.
    *   Ye functions `AttendanceRecorder` se data `pandas DataFrame` ki shakal mein laate hain aur `st.dataframe()` se display hota hai.
    *   Student-wise attendance mein summary bhi calculate hoti hai (total days, present days, absent days).
9.  **Dashboard:**
    *   `system_manager.get_overall_stats()` call hota hai jo total students, total attendance records, recent attendance jaisi stats calculate karta hai.
    *   Ye stats `st.metric()` aur `st.bar_chart()` se graph ki shakal mein dikhayi jati hain.
10. **Logout:** Sidebar mein `Logout` button press karne par `auth_manager.logout()` call hota hai aur aap wapas login screen par aa jate hain.

## Khaas Features (Special Functions/Features)

*   **Modular Design:** Code ko chhote chhote hisson (files aur classes) mein baanta gaya hai. Har hisse ka apna khaas kaam hai (maslan `models.py` data structure define karta hai, `managers.py` data read/write karta hai). Isse code ko samajhna aur maintain karna aasan ho jata hai.
*   **Data Persistence (File-based):** Data ko files (`.txt`) mein save kiya jata hai, isliye jab aap app close karte hain aur phir chalate hain, to aapka data wahin ka wahin rehta hai. Iske liye koi database setup ki zaroorat nahi.
*   **Session Management:** `st.session_state` use karke user ka login status aur doosri information app ke restart hone tak ya tab tak jab tak user logout na kare, yaad rehti hai.
*   **Data Consistency:** Jab aap koi student delete karte hain, to uski saari attendance records bhi automatically delete ho jati hain. Isi tarah, agar aap student ka naam badalte hain, to uski purani attendance records mein bhi naam update ho jata hai. Ye system ko reliable banata hai.
*   **User-Friendly Interface:** Streamlit ki wajah se app ka UI bohot simple aur easy to use hai, forms, buttons, checkboxes ke saath.

## Viva Ke Liye Basic Sawal Jawab (Basic Q&A for Viva)

**Q1: Aapke project ka naam aur maqsad kya hai?**
**A1:** Mere project ka naam "Attendance Management System" hai. Iska maqsad students ki hajri (attendance) ko digital tareeqe se manage karna hai. Ismein students ko add kar sakte hain, unki daily attendance mark kar sakte hain, aur purani attendance records ko dekh sakte hain.

**Q2: Aapne is project mein kaunsi programming language aur framework use ki hai?**
**A2:** Maine is project mein Python programming language use ki hai, aur user interface (GUI) banane ke liye Streamlit framework istemal kiya hai. Data ko organize karne aur files mein save karne ke liye Pandas bhi use kiya hai.

**Q3: Data kahan store hota hai? Kya aapne database use kiya hai?**
**A3:** Nahi, maine koi proper database (jaise MySQL ya SQLite) use nahi kiya hai. Data simple text files (`.txt`) mein store hota hai, jo `data` folder ke andar hoti hain. `students.txt` mein students ka record hota hai aur `attendance.txt` mein attendance ka record.

**Q4: `main.py` file ka kya role hai?**
**A4:** `main.py` hamari application ka main file hai. Ye Streamlit app ko run karta hai aur user ko jo kuch bhi screen par dikhta hai (login form, navigation, data tables), wo sab isi file mein design aur display hota hai. Ye doosri files (`auth.py`, `system.py`) ke functions ko call karke kaam karta hai.

**Q5: `auth.py` file kis kaam aati hai?**
**A5:** `auth.py` file admin authentication (login/logout) ko handle karti hai. Ye check karti hai ke user ne sahi username aur password daala hai ya nahi. Jab admin login ho jata hai, tabhi system ke main features access kiye ja sakte hain.

**Q6: `models.py` aur `managers.py` files ka kya kaam hai?**
**A6:**
*   **`models.py`:** Ye data ke "blueprints" hain. Ismein `Student` aur `AttendanceRecord` jaisi classes hain jo define karti hain ke hamara data kis structure mein hoga (maslan, student ka ID aur naam).
*   **`managers.py`:** Ye "data ke dekhbhaal karne wale" hain. Ismein `StudentManager` aur `AttendanceRecorder` jaisi classes hain jo actual mein files se data read karte hain, usko update karte hain aur wapas files mein save karte hain. Ye models ko use karte hain.

**Q7: `system.py` file ka kya maqsad hai?**
**A7:** `system.py` file "poore system ka master control" hai. Ye `StudentManager` aur `AttendanceRecorder` (jo `managers.py` mein hain) ko ek saath combine karti hai. Ye `main.py` ko high-level functions provide karti hai. Jab aap "add student" jaise koi kaam karte hain, to `main.py` `system.py` ko batata hai, aur `system.py` phir aage related manager ko call karta hai. Ye data consistency bhi dekhta hai (maslan student delete karne par uski attendance bhi delete karna).

**Q8: Aapne `st.session_state` kyun use kiya hai?**
**A8:** `st.session_state` Streamlit mein bohot important hai. Streamlit har interaction (jaise button press) par poori app ko dobara run karta hai. `st.session_state` ki wajah se hum variables ki values (jaise login status, ya selected student ka naam) ko "session" ke dauran yaad rakh sakte hain, taake har bar app reload hone par wo values khatam na ho jayen.
