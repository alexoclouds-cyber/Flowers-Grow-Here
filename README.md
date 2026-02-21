from PyQt5.QtCore import Qt, QTime, QTimer
from PyQt5.QtWidgets import QButtonGroup, QApplication, QGroupBox, QPushButton, QWidget, QLabel, QHBoxLayout, QVBoxLayout, QMessageBox, QRadioButton
from random import shuffle
start_time = QTime(0, 0, 0)
final = []
class Question():
    def __init__(self, question, ans1, ans2, ans3, ans4, ans5):
        self.question = question
        self.ans1 = ans1
        self.ans2 = ans2
        self.ans3 = ans3
        self.ans4 = ans4
        self.ans5 = ans5

def show_result():
    RadioGroupBox.hide()
    StatisticsGroup.hide()
    RadioGroupBox2.show()
    ansButton.setText('Next Question')
def show_question():
    RadioGroupBox2.hide()
    RadioGroupBox.show()
    StatisticsGroup.hide()
    ansButton.setText('Answer')
    RadioGroup.setExclusive(False)
    rbtn_1.setChecked(False)
    rbtn_2.setChecked(False)
    rbtn_3.setChecked(False)
    rbtn_4.setChecked(False)
    rbtn_5.setChecked(False)
    RadioGroup.setExclusive(True)
def start_test():
    if ansButton.text() == 'Answer':
        check_answer()
    elif ansButton.text() == 'Restart':
        restart()
    else:
        next_question()
def ask(q: Question):
    shuffle(answers)
    answers[0].setText(q.ans1)
    answers[1].setText(q.ans2)
    answers[2].setText(q.ans3)
    answers[3].setText(q.ans4)
    answers[4].setText(q.ans5)
    ib_question.setText(q.question)
    show_question()
def show_correct(res):        
    print('Stats')
    print('- All Questions:', main_win.total)
    print('- Right answers:', main_win.right)
    print('Rating', main_win.right/main_win.total*100, "%")
    show_result()
def check_answer():
    if answers[0].isChecked():
        final.append(answers[0].text())
        show_correct('Submited')
    elif answers[1].isChecked():
        final.append(answers[1].text())
        show_correct('Submited')
    elif answers[2].isChecked():
        final.append(answers[2].text())
        show_correct('Submited')
    elif answers[3].isChecked():
        final.append(answers[3].text())
        show_correct('Submited')
    elif answers[4].isChecked():
        final.append(answers[4].text())
        show_correct('Submited')
    else:
        answer_choice()
def answer_choice():
    choice_win = QMessageBox()
    choice_win.setText('Choose Answer!!!')
    choice_win.exec()
def next_question():
    if main_win.count == 0:
        start_timer()
    if main_win.count > len(questions)-1:
        show_statistics()
    else:
        main_win.total +=1
        q = questions[main_win.count]
        main_win.count += 1
        ask(q)
def restart():
    global start_time
    main_win.total = 0
    main_win.count = 0
    main_win.right = 0

    start_time = QTime(0, 0)
    start_timer()
    next_question()
    final.clear
def show_statistics():
    stop_timer()
    RadioGroupBox.hide()
    RadioGroupBox2.hide()
    StatisticsGroup.show()
    ansButton.setText('Restart')
    ib_question.setText('')
    ib_total_question.setText('- Your best flower is: ' + best())
    ib_rating.setText('- How much do you suite your chosen flower: ' + str(checking()))
    ib_rating2.setText('- Probobility for your result: ' + '1/390,625')
    spent_time.setText('- Time:' + start_time.toString("mm:ss"))
questions = [
    Question('What flower do you want to be?', 'sunflower', 'rose', 'cactus', 'water lili', 'snowdrop'),
    Question('What is your fav color?', 'yellow', 'red', 'green', 'pink', 'white'),
    Question('What temperature do you like in degree C?', '18-30', '15-26', '35-40', '20-28', '0-15'),
    Question('What personality are you?', 'happy', 'elegant', 'crazy', 'calm', 'sleepy'),
    Question('What is your fav weather?', 'sunny', 'light rain', 'desert', 'tropical', 'cold'),
    Question('What continent do you like?', 'America', 'Europe', 'Australia', 'Asia', 'Antarctica'),
    Question('What height ranhe would you like as being a flower', '5-10ft', '1.5 - 5ft', '6-20ft', '15-30cm', '10-25cm'),
    Question('What animal do you like?', 'lion', 'flamingo', 'crocodile', 'butterfly', 'bunny'),
    Question('What is your fav accessory?', 'converse', 'pearl neckless', 'sunglasses', 'crown', 'blanket')
    ]
def checking():
    global final
    d = 0
    if final[0] == 'rose':
        for i in final:
            if i in ['red', '15-26', 'elegant', 'light rain', 'Europe', '1.5 - 5ft', 'flamingo', 'pearl neckless']:
                d+=1
    elif final[0] == 'sunflower':
        for i in final:
            if i in ['yellow', '18-30', 'happy', 'sunny', 'America', '5-10ft', 'lion', 'converse']:
                d+=1
    elif final[0] == 'cactus':
        for i in final:
            if i in ['green', '35-40', 'crazy', 'desert', 'Australia', '6-20ft', 'crocodile', 'sunglasses']:
                d+=1
    elif final[0] == 'water lili':
        for i in final:
            if i in ['pink', '20-28', 'calm', 'cold', 'Asia', '15-30cm', 'butterfly', 'crown']:
                d+=1
            else:
                d+=0            
    elif final[0] == 'snowdrop':
        for i in final:
            if i in ['white', '0-15', 'sleepy', 'tropical', 'Antarctica', '15-30cm', 'bunny', 'blanket']:
                d+=1
    a = round(d*100//7,2)
    return a
def best():
    global final
    s = [0, 0, 0, 0, 0]
    for i in final:
        if i in ['red', '15-26', 'elegant', 'light rain', 'Europe', '1.5 - 5ft', 'flamingo', 'pearl neckless']:
            s[2]+=1
        elif i in ['yellow', '18-30', 'happy', 'sunny', 'America', '5-10ft', 'lion', 'converse']:
            s[0]+=1
        elif i in ['green', '35-40', 'crazy', 'desert', 'Australia', '6-20ft', 'crocodile', 'sunglasses']:
            s[1]+=1
        elif i in ['pink', '20-28', 'calm', 'cold', 'Asia', '15-30cm', 'butterfly', 'crown']:
            s[3]+=1
        elif i in ['white', '0-15', 'sleepy', 'tropical', 'Antarctica', '15-30cm', 'bunny', 'blanket']:
            s[4]+=1
    a = max(s)
    m = ''
    if s[0] == a:
        m += 'rose '
    if s[1] == a:
        m+='sunflower '
    if s[2] == a:
        m+='cactus '
    if s[3] == a:
        m+='water lili '
    if s[4] == a:
        m+='snowdrop '
    return m 

app = QApplication([])
main_win =  QWidget()
main_win.setFixedSize(600, 500)
main_win.setWindowTitle('Your life as a flower')

timer = QTimer()
start_time = QTime(0,0)
time_label = QLabel('Time: 00:00')
time_label.setAlignment(Qt.AlignRight | Qt.AlignCenter)

def update_timer():
    global start_time
    start_time = start_time.addSecs(1)
    time_label.setText(f'Time:{start_time.toString("mm:ss")}')

def start_timer():
    global start_time
    timer.stop()  
    start_time = QTime(0, 0)
    time_label.setText('Time: 00:00')
    try:
        timer.timeout.disconnect() 
    except TypeError:
        pass  
    timer.timeout.connect(update_timer)
    timer.start(1000)

def stop_timer():
    timer.stop()

app.setStyleSheet("""
    QWidget { 
        background: #ffb3da;
        color: #660033;
        font-famyly: Arial;
        font-size: 12pt
    }
    QGroupBox {
        background: rgba(255, 255, 153, 0.2);
        bolder: 2px solid white;
        border-radius: 10px;
        margin-top: 1ex;
        font-weight: bold;
    }
    QGoupBox::title {
        subcontrol-origin: margin;
        left: 10px;
        padding:  0 3px;
        color: white;
    }
    QRadioButton {
        padding: 5px;
        background: transparent;
    }
    QRadioButton::indicator {
        width: 20px;
        height: 20px;
        border-radius: 10px;
        border: 2px solid #ff00ff;
    }
    QRadioButton::indicator:checked{
        background: white;
        border: 2px solid #99bbff;
    }
    QPushButton {
        background: qlineargradient(x1:0, y1:0, x2:0, y2:1, stop:0 #ff0066, stop:1 #ffb3d1);
        border: 2px solid #ff00ff;
        border-radius: 10px;
        padding: 10px 20px;
        font-weight: bold;
        min-width: 160px;
    }
    QPushButton:hover {
        background: qlineargradient(x1:0, y1:0, x2:0, y2:1, stop:0 #1aa3ff, stop:1 #005c99) 

    }
    QPushButton:pressed {
        background: qlineargradient(x1:0, y1:0, x2:0, y2:1, stop:0 #ffff99, stop:1 #ffff4d)

    }
    QLabel {
        font-size: 14pt;
        background: transparent;

    }
    QLabel#ib_question{
        font-size: 16pt;
        font-weight: bold;
        background: transparent;
    }
    """)
ib_question = QLabel('?')
ib_question.setWordWrap(True)
ib_question.setObjectName("ib_question")
ansButton = QPushButton('Answer')

RadioGroupBox = QGroupBox('Options')
rbtn_1 = QRadioButton('1')
rbtn_2 = QRadioButton('2')
rbtn_3 = QRadioButton('3')
rbtn_4 = QRadioButton('4')
rbtn_5 = QRadioButton('5')
answers = [rbtn_1, rbtn_2, rbtn_3, rbtn_4, rbtn_5]

RadioGroup = QButtonGroup()
RadioGroup.addButton(rbtn_1)
RadioGroup.addButton(rbtn_2)
RadioGroup.addButton(rbtn_3)
RadioGroup.addButton(rbtn_4)
RadioGroup.addButton(rbtn_5)

layout_a1 = QHBoxLayout()
layout_a2 = QVBoxLayout()
layout_a3 = QVBoxLayout()

layout_a2.addWidget(rbtn_1)
layout_a2.addWidget(rbtn_2)
layout_a3.addWidget(rbtn_3)
layout_a3.addWidget(rbtn_4)
layout_a3.addWidget(rbtn_5)
layout_a1.addLayout(layout_a2)
layout_a1.addLayout(layout_a3)

RadioGroupBox.setLayout(layout_a1)

RadioGroupBox2 = QGroupBox('Test results')
text = QLabel('submited')
text2 = QLabel('yes')

layoutB = QVBoxLayout()
layoutB.addStretch(1)
layoutB.addWidget(text, alignment = (Qt.AlignLeft | Qt.AlignTop))
layoutB.addWidget(text2, alignment = Qt.AlignCenter, stretch = 2)
RadioGroupBox2.setLayout(layoutB)

StatisticsGroup = QGroupBox("  Stats")
ib_total_question = QLabel('Amount of Questions')
ib_rating = QLabel('Rating')
ib_rating2 = QLabel('Rating2')
spent_time = QLabel('- Time spent:' + start_time.toString("mm:ss"))
layout_stas = QVBoxLayout()
#layout_stas.addStretch(1)
layout_stas.addWidget(ib_total_question, alignment = Qt.AlignLeft)
layout_stas.addWidget(ib_rating, alignment = Qt.AlignLeft)
layout_stas.addWidget(spent_time, alignment = Qt.AlignLeft)
StatisticsGroup.setLayout(layout_stas)

main_lay_H1 = QHBoxLayout()
main_lay_H2 = QHBoxLayout()
main_lay_H3 = QHBoxLayout()

main_lay_V = QVBoxLayout()

main_lay_H1.addWidget(ib_question, alignment=(Qt.AlignHCenter | Qt.AlignVCenter))
main_lay_H1.addStretch(1)
main_lay_H1.addWidget(time_label)

main_lay_H1.addWidget(ib_question, alignment = Qt.AlignCenter)
main_lay_H2.addWidget(RadioGroupBox)
main_lay_H2.addWidget(RadioGroupBox2)
main_lay_H2.addWidget(StatisticsGroup)
StatisticsGroup.hide()
RadioGroupBox2.hide()

main_lay_H3.addStretch(1)
main_lay_H3.addWidget(ansButton, stretch = 4)
main_lay_H3.addStretch(1)

main_lay_V.addLayout(main_lay_H1, stretch = 2)
main_lay_V.addLayout(main_lay_H2, stretch = 8)
main_lay_V.addStretch(1)
main_lay_V.addLayout(main_lay_H3, stretch = 1)
main_lay_V.addStretch(1)
main_lay_V.setSpacing(5)
main_win.setLayout(main_lay_V)

main_win.total = 0
main_win.right = 0
main_win.count = 0
next_question()
ansButton.clicked.connect(start_test)

start_timer()
main_win.show()
app.exec_()
