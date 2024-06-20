- 👋 Hi, I’m @panagiotis1234567
- 👀 I’m interested in cyber security, game developing and web engineering
- 🌱 I’m currently learning python and Unix
- 💞️ I’m looking to collaborate on a cyber security company
- 📫 How to reach me panagiotistt1@gmail.com
- 😄 Pronouns: he\him
- ⚡ Fun fact: wanabe ethical\white hat hacker

<!---
panagiotis1234567/panagiotis1234567 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
---> notepad in python :











import sys
from PyQt5.QtWidgets import (
    QApplication, QMainWindow, QTextEdit, QAction, QFileDialog, QColorDialog,
    QMessageBox, QFontDialog, QToolBar
)
from PyQt5.QtGui import QIcon, QPainter, QPen, QTextCursor, QTextCharFormat
from PyQt5.QtCore import Qt, QPoint

class Notepad(QMainWindow):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setWindowTitle('Notepad')
        self.setGeometry(100, 100, 800, 600)

        self.textEdit = QTextEdit(self)
        self.setCentralWidget(self.textEdit)

        menubar = self.menuBar()

        fileMenu = menubar.addMenu('File')
        newAction = QAction('New', self)
        newAction.setShortcut('Ctrl+N')
        newAction.triggered.connect(self.newFile)
        fileMenu.addAction(newAction)

        openAction = QAction('Open', self)
        openAction.setShortcut('Ctrl+O')
        openAction.triggered.connect(self.openFile)
        fileMenu.addAction(openAction)

        saveAction = QAction('Save', self)
        saveAction.setShortcut('Ctrl+S')
        saveAction.triggered.connect(self.saveFile)
        fileMenu.addAction(saveAction)

        saveAsAction = QAction('Save As...', self)
        saveAsAction.triggered.connect(self.saveFileAs)
        fileMenu.addAction(saveAsAction)

        exitAction = QAction('Exit', self)
        exitAction.setShortcut('Ctrl+Q')
        exitAction.triggered.connect(self.close)
        fileMenu.addAction(exitAction)

        editMenu = menubar.addMenu('Edit')
        cutAction = QAction('Cut', self)
        cutAction.setShortcut('Ctrl+X')
        cutAction.triggered.connect(self.textEdit.cut)
        editMenu.addAction(cutAction)

        copyAction = QAction('Copy', self)
        copyAction.setShortcut('Ctrl+C')
        copyAction.triggered.connect(self.textEdit.copy)
        editMenu.addAction(copyAction)

        pasteAction = QAction('Paste', self)
        pasteAction.setShortcut('Ctrl+V')
        pasteAction.triggered.connect(self.textEdit.paste)
        editMenu.addAction(pasteAction)

        undoAction = QAction('Undo', self)
        undoAction.setShortcut('Ctrl+Z')
        undoAction.triggered.connect(self.textEdit.undo)
        editMenu.addAction(undoAction)

        redoAction = QAction('Redo', self)
        redoAction.setShortcut('Ctrl+Y')
        redoAction.triggered.connect(self.textEdit.redo)
        editMenu.addAction(redoAction)

        selectAllAction = QAction('Select All', self)
        selectAllAction.setShortcut('Ctrl+A')
        selectAllAction.triggered.connect(self.textEdit.selectAll)
        editMenu.addAction(selectAllAction)

        formatMenu = menubar.addMenu('settings')

        bgColorAction = QAction('Change Background Color', self)
        bgColorAction.triggered.connect(self.changeBackgroundColor)
        formatMenu.addAction(bgColorAction)

        highlightAction = QAction('Highlight Text', self)
        highlightAction.triggered.connect(self.highlightText)
        formatMenu.addAction(highlightAction)

        changeFontAction = QAction('Change Font', self)
        changeFontAction.triggered.connect(self.changeFont)
        formatMenu.addAction(changeFontAction)

        changeTextColorAction = QAction('Change Text Color', self)
        changeTextColorAction.triggered.connect(self.changeTextColor)
        formatMenu.addAction(changeTextColorAction)

        self.current_file = ''

    def newFile(self):
        if self.confirmSaveChanges():
            self.textEdit.clear()
            self.current_file = ''

    def openFile(self):
        if self.confirmSaveChanges():
            options = QFileDialog.Options()
            file_name, _ = QFileDialog.getOpenFileName(self, "Open File", "", "Text Files (*.txt);;All Files (*)", options=options)
            if file_name:
                with open(file_name, 'r') as file:
                    self.textEdit.setText(file.read())
                self.current_file = file_name

    def saveFile(self):
        if self.current_file:
            with open(self.current_file, 'w') as file:
                file.write(self.textEdit.toPlainText())
        else:
            self.saveFileAs()

    def saveFileAs(self):
        options = QFileDialog.Options()
        file_name, _ = QFileDialog.getSaveFileName(self, "Save File As", "", "Text Files (*.txt);;All Files (*)", options=options)
        if file_name:
            self.current_file = file_name
            with open(file_name, 'w') as file:
                file.write(self.textEdit.toPlainText())

    def confirmSaveChanges(self):
        if not self.textEdit.document().isModified():
            return True

        response = QMessageBox.question(self, 'Message', "You have unsaved changes. Do you want to save them?", 
                                        QMessageBox.Yes | QMessageBox.No | QMessageBox.Cancel, QMessageBox.Cancel)
        if response == QMessageBox.Cancel:
            return False
        if response == QMessageBox.Yes:
            self.saveFile()
        return True

    def changeBackgroundColor(self):
        color = QColorDialog.getColor()
        if color.isValid():
            self.textEdit.setStyleSheet(f"background-color: {color.name()};")

    def highlightText(self):
        color = QColorDialog.getColor()
        if color.isValid():
            cursor = self.textEdit.textCursor()
            fmt = QTextCharFormat()
            fmt.setBackground(color)
            cursor.mergeCharFormat(fmt)
            self.textEdit.mergeCurrentCharFormat(fmt)

    def changeFont(self):
        font, ok = QFontDialog.getFont()
        if ok:
            self.textEdit.setFont(font)

    def changeTextColor(self):
        color = QColorDialog.getColor()
        if color.isValid():
            cursor = self.textEdit.textCursor()
            fmt = QTextCharFormat()
            fmt.setForeground(color)
            cursor.mergeCharFormat(fmt)
            self.textEdit.mergeCurrentCharFormat(fmt)

    def closeEvent(self, event):
        if self.confirmSaveChanges():
            event.accept()
        else:
            event.ignore()

def main():
    app = QApplication(sys.argv)
    notepad = Notepad()
    notepad.show()
    sys.exit(app.exec_())

if __name__ == '__main__':
    main()


