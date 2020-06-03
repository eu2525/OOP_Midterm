#include <iostream>
#include <fstream>
#include <string>
#include <vector>
using namespace std;

// Utils
int findStudentById(vector<string> studentIds, string id);
int findLectureByCode(vector<string> lectureCodes, string code);
void deleteElement(vector<string>& v, int index);

// File read
void readStudentFile(vector<string>& studentIds, vector<string>& passwords, vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes);
void readLectureFile(vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits);

// File write
void writeStudentFile(const vector<string>& studentIds, const vector<string>& passwords, const vector<string>& names, const vector<int>& credits, const vector<vector<string>>& appliedLectureCodes);
void writeLectureFile(const vector<string>& lectureCodes, const vector<string>& lectureNames, const vector<int>& lectureCredits, const vector<int>& limits);

// Get user input
string getInputId();
string getInputPassword();

int studentLogin(const vector<string>& studentIds, const vector<string>& passwords);
bool adminLogin();
int login(const vector<string>& studentIds, const vector<string>& passwords);

// Common
void printLectures(const vector<vector<string>>& appliedLectureCodes, const vector<string>& lectureCodes, const vector<string>& lectureNames, const vector<int>& lectureCredits, const vector<int>& limits, const int& user = -100);


// Admin
void addStudent(vector<string>& studentIds, vector<string>& passwords, vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes);
void addLecture(vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits);
void deleteLecture(vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits, vector<int>& credits, vector<vector<string>>& appliedLectureCodes);
int runAdmin(vector<string>& studentIds, vector<string>& passwords, vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes, vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits);


void printStudent(const vector<string>& studentIds, const vector<string>& names, const vector<int>& credits, const int& user);
void applyLecture(const vector<string>& studentIds, const vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes, const vector<string>& lectureCodes, const vector<string>& lectureNames, const vector<int>& lectureCredits, vector<int>& limits, const int& user);
void disapplyLecture(const vector<string>& studentIds, const vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes, const vector<string>& lectureCodes, const vector<string>& lectureNames, const vector<int>& lectureCredits, vector<int>& limits, const int& user);
void changePassword(vector<string>& passwords, const int& user);
int runStudent(vector<string>& studentIds, vector<string>& passwords, vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes, vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits, int user);

int main() {
	int user = -1; //user index
	int status = -1;

	// Student Info
	vector<string> studentIds;
	vector<string> passwords;
	vector<string> names;
	vector<int> credits;
	vector<vector<string>> appliedLectureCodes;

	// Lecture Info
	vector<string> lectureCodes;
	vector<string> lectureNames;
	vector<int> lectureCredits;
	vector<int> limits;

	// Read from files
	readStudentFile(studentIds, passwords, names, credits, appliedLectureCodes);
	readLectureFile(lectureCodes, lectureNames, lectureCredits, limits);



	// Login phase
	while (status == -1) {
		user = login(studentIds, passwords);

		if (user == -999) { // Login fail
			break;
		}
		else if (user == -1) { // Exit command
			break;
		}
		else if (user == -100) { // Admin login success
			status = runAdmin(studentIds, passwords, names, credits, appliedLectureCodes, lectureCodes, lectureNames, lectureCredits, limits);
		}
		else { // Student login success
			status = runStudent(studentIds, passwords, names, credits, appliedLectureCodes, lectureCodes, lectureNames, lectureCredits, limits, user);
		}
	}

	// Write to files
	writeStudentFile(studentIds, passwords, names, credits, appliedLectureCodes);
	writeLectureFile(lectureCodes, lectureNames, lectureCredits, limits);



	return 0;
}

int findStudentById(vector<string> studentIds, string id) {
	for (unsigned i = 0; i < studentIds.size(); i++) {
		if (studentIds[i] == id)
			return i;
	}
	return -1;
}

int findLectureByCode(vector<string> lectureCodes, string code) {
	for (unsigned i = 0; i < lectureCodes.size(); i++) {
		if (lectureCodes[i] == code)
			return i;
	}
	return -1;
}

void deleteElement(vector<string>& v, int index) {
	v.erase(v.begin() + index);
}


void readStudentFile(vector<string>& studentIds, vector<string>& passwords, vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes) {
	ifstream fin("students.txt");
	string studentId, password, name, appliedLectureCo;
	vector<string> appliedLectureCode;
	int credit, lecturenum;
	int i = 0;
	while (true) {

		if (i % 5 == 0) {
			fin >> studentId;
			if (!fin)
				break;
			else
				studentIds.push_back(studentId);
		}
		else if (i % 5 == 1) {
			fin >> password;
			passwords.push_back(password);
		}
		else if (i % 5 == 2) {
			fin >> name;
			names.push_back(name);
		}
		else if (i % 5 == 3) {
			fin >> credit;
			credits.push_back(credit);
		}
		else if (i % 5 == 4) {
			fin >> lecturenum;
			if (lecturenum == 0) {
				vector<string> t;
				appliedLectureCodes.push_back(t);
			}
			else {
				for (int i = 0; i < lecturenum; i++) {
					fin >> appliedLectureCo;
					appliedLectureCode.push_back(appliedLectureCo);
				}
				appliedLectureCodes.push_back(appliedLectureCode);
			}
		}
		i++;
	}
	fin.close();
}

void readLectureFile(vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits) {
	ifstream fin("lectures.txt");
	string lectureCode, lectureName;
	int lectureCredit, limit;
	int i = 0;
	while (true) {

		if (i % 4 == 0) {
			fin >> lectureCode;
			if (!fin)
				break;
			else
				lectureCodes.push_back(lectureCode);
		}
		else if (i % 4 == 1) {
			fin >> lectureName;
			lectureNames.push_back(lectureName);
		}
		else if (i % 4 == 2) {
			fin >> lectureCredit;
			lectureCredits.push_back(lectureCredit);
		}
		else if (i % 4 == 3) {
			fin >> limit;
			limits.push_back(limit);
		}

		i++;
	}
	fin.close();
}

void writeStudentFile(const vector<string>& studentIds, const vector<string>& passwords, const vector<string>& names, const vector<int>& credits, const vector<vector<string>>& appliedLectureCodes) {
	ofstream fout("students.txt");
	for (unsigned i = 0; i < studentIds.size(); i++) {
		fout << studentIds[i] << "\t" << passwords[i] << "\t" << names[i] << "\t" << credits[i] << "\t" << appliedLectureCodes[i].size() << "\t";
		for (string s : appliedLectureCodes[i]) {
			fout << s << " ";
		}
		fout << endl;
	}
	fout.close();
}

void writeLectureFile(const vector<string>& lectureCodes, const vector<string>& lectureNames, const vector<int>& lectureCredits, const vector<int>& limits) {
	ofstream fout("lectures.txt");
	for (unsigned i = 0; i < lectureCodes.size(); i++) {
		fout << lectureCodes[i] << "\t" << lectureNames[i] << "\t" << lectureCredits[i] << "\t" << limits[i] << endl;
	}
	fout.close();
}


string getInputId() {
	string id;
	cout << "아이디 : ";
	cin >> id;
	return id;
}

string getInputPassword() {
	string pw;
	cout << "비밀번호 : ";
	cin >> pw;
	return pw;
}


int studentLogin(const vector<string>& studentIds, const vector<string>& passwords) {
	string id, pw;
	int index;
	id = getInputId();
	pw = getInputPassword();
	index = findStudentById(studentIds, id);
	if (index != -1 && passwords[index] == pw) {
		return index;
	}
	else {
		return -1;
	}

}

bool adminLogin() {
	string adminid, adminpw;
	adminid = getInputId();
	adminpw = getInputPassword();
	if (adminid == "admin" && adminpw == "admin") {
		return true;
	}
	else
		return false;
}

int login(const vector<string>& studentIds, const vector<string>& passwords) {
	int input;
	cout << "------------------------------------------------------------------------" << endl;
	cout << "1. 학생 로그인" << endl;
	cout << "2. 관리자 로그인" << endl;
	cout << "0. 종료" << endl;
	cout << "------------------------------------------------------------------------" << endl;
	cout << ">>";
	cin >> input;
	if (input == 1) {
		for (int k = 0; k < 3; k++) {
			int s1 = studentLogin(studentIds, passwords);

			if (s1 == -1 && k != 2) {
				cout << "로그인 " << (k + 1) << "회 실패 (3회 실패시 종료)" << endl;
			}

			else if (s1 == -1 && k == 2) {
				cout << "로그인 " << (k + 1) << "회 실패 (3회 실패시 종료)" << endl;
				cout << "3회 실패하여 종료합니다." << endl;
				return -999;
			}

			else if (s1 != -1) {
				return s1;
			}
		}
	}
	else if (input == 2) {
		for (int k = 0; k < 3; k++) {
			bool aL = adminLogin();
			if (aL == false && k != 2) {
				cout << "로그인 " << (k + 1) << "회 실패 (3회 실패시 종료)" << endl;
			}

			else if (aL == false && k == 2) {
				cout << "로그인 " << (k + 1) << "회 실패 (3회 실패시 종료)" << endl;
				cout << "3회 실패하여 종료합니다." << endl;
				return -999;
			}

			else if (aL == true) {
				return -100;
			}
		}

	}
	else if (input == 0) {
		return -1;
	}
}

void printLectures(const vector<vector<string>>& appliedLectureCodes, const vector<string>& lectureCodes, const vector<string>& lectureNames, const vector<int>& lectureCredits, const vector<int>& limits, const int& user) {
	cout << "------------------------------------------------------------------------" << endl;
	cout << "과목 코드" << "\t" << "강의명" << "\t" << "학점" << "\t" << "수강가능인원" << endl;
	cout << "------------------------------------------------------------------------" << endl;

	if (user == -100) {
		for (unsigned i = 0; i < lectureCodes.size(); i++) {
			cout << lectureCodes[i] << "\t" << lectureNames[i] << "\t" << lectureCredits[i] << "\t" << limits[i] << endl;
		}
		cout << "------------------------------------------------------------------------" << endl;
	}

	else {
		for (string code : appliedLectureCodes[user]) {
			int codenum = findLectureByCode(lectureCodes, code);
			cout << lectureCodes[codenum] << "\t" << lectureNames[codenum] << "\t" << lectureCredits[codenum] << "\t" << limits[codenum] << endl;
		}
		cout << "------------------------------------------------------------------------" << endl;
	}

}

void printStudent(const vector<string>& studentIds, const vector<string>& names, const vector<int>& credits, const int& user) {
	cout << "------------------------------------------------------------------------" << endl;
	cout << "학번 : " << studentIds[user] << "\t" << "이름 : " << names[user] << "\t" << "수강가능학점 : " << credits[user] << endl;
	cout << "------------------------------------------------------------------------" << endl;

}

void applyLecture(const vector<string>& studentIds, const vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes, const vector<string>& lectureCodes, const vector<string>& lectureNames, const vector<int>& lectureCredits, vector<int>& limits, const int& user) {
	string inputstr, lecName;
	printStudent(studentIds, names, credits, user);
	printLectures(appliedLectureCodes, lectureCodes, lectureNames, lectureCredits, limits, -100);
	while (true) {
		cout << "신청할 과목 코드(0: 뒤로가기) >> ";
		cin >> inputstr;
		int num = 0;
		bool ri = true, li = false;

		if (inputstr == "0") {
			break;
		}
		int index;
		index = findLectureByCode(lectureCodes, inputstr);

		for (unsigned k = 0; k < appliedLectureCodes[user].size(); k++) {
			for (unsigned j = 0; j < lectureCodes.size(); j++) {
				if (appliedLectureCodes[user][k] == lectureCodes[j]) {
					lecName = lectureNames[j];
					if (lecName == lectureNames[index])
						li = true;
				}
			}
		}

		for (string elem : appliedLectureCodes[user]) {
			if (elem == lectureCodes[index]) {
				ri = false;
			}
			else
				ri = true;
		}


		if (limits[index] == 0) {
			cout << "수강정원이 꽉 찼습니다." << endl;
		}
		else if (credits[user] - lectureCredits[index] < 0) {
			cout << "수강가능학점이 부족합니다." << endl;
		}
		else if (ri == false) {
			cout << "이미 해당 과목을 신청했습니다." << endl;
		}
		else if (li == true) {
			cout << "이미 동일명의 과목을 신청했습니다." << endl;
		}

		else {
			cout << "[" << inputstr << "]" << lectureNames[index] << "을(를) 성공적으로 신청하였습니다." << endl;
			appliedLectureCodes[user].push_back(lectureCodes[index]);
			limits[index] = (limits[index] - 1);
			credits[user] = credits[user] - lectureCredits[index];
		}
		system("PAUSE");
	}

}

void disapplyLecture(const vector<string>& studentIds, const vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes, const vector<string>& lectureCodes, const vector<string>& lectureNames, const vector<int>& lectureCredits, vector<int>& limits, const int& user) {
	string ipstr;
	printStudent(studentIds, names, credits, user);
	printLectures(appliedLectureCodes, lectureCodes, lectureNames, lectureCredits, limits, user);
	while (true) {
		cout << "철회할 과목 코드(0: 뒤로가기) >> ";
		cin >> ipstr;
		if (ipstr == "0") {
			break;
		}
		int index = findLectureByCode(lectureCodes, ipstr);

		vector<string> vecelem = appliedLectureCodes[user];
		int ind = findLectureByCode(vecelem, ipstr);
		if (ind == -1) {
			cout << "과목 코드가 옳바르지 않습니다." << endl;
		}
		else {
			cout << "[" << ipstr << "]" << lectureNames[index] << "을(를) 철회하였습니다." << endl;
			deleteElement(appliedLectureCodes[user], ind);
			limits[index] = (limits[index] + 1);
			credits[user] = credits[user] + lectureCredits[index];
		}
		system("PAUSE");
	}
}

void changePassword(vector<string>& passwords, const int& user) {
	string epw, new_pw;
	cout << "------------------------------------------------------------------------" << endl;
	cout << "본인 확인을 위해 한번 더 비밀번호를 한 번 더 입력해주세요." << endl;
	cout << "비밀번호 : ";
	cin >> epw;
	cout << "------------------------------------------------------------------------" << endl;

	if (passwords[user] == epw) {
		cout << "새로 설정할 비밀번호를 입력하세요." << endl;
		cout << "비밀번호 : ";
		cin >> new_pw;
		passwords[user] = new_pw;
		cout << "변경되었습니다." << endl;
		cout << "------------------------------------------------------------------------" << endl;
	}
	else {
		cout << "------------------------------------------------------------------------" << endl;
		cout << "비밀번호가 일치하지 않습니다." << endl;
	}

}

int runStudent(vector<string>& studentIds, vector<string>& passwords, vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes, vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits, int user) {
	while (true) {
		int inputnum;
		cout << "------------------------------------------------------------------------" << endl;
		cout << "1. 수강 신청" << endl;
		cout << "2. 수강 현황" << endl;
		cout << "3. 수강 철회" << endl;
		cout << "4. 비밀번호 변경" << endl;
		cout << "5. 로그아웃" << endl;
		cout << "0. 종료" << endl;
		cout << "------------------------------------------------------------------------" << endl;
		cout << ">>";
		cin >> inputnum;

		if (inputnum == 1) {
			applyLecture(studentIds, names, credits, appliedLectureCodes, lectureCodes, lectureNames, lectureCredits, limits, user);

		}
		else if (inputnum == 2) {
			printStudent(studentIds, names, credits, user);
			printLectures(appliedLectureCodes, lectureCodes, lectureNames, lectureCredits, limits, user);

		}
		else if (inputnum == 3) {
			disapplyLecture(studentIds, names, credits, appliedLectureCodes, lectureCodes, lectureNames, lectureCredits, limits, user);
		}
		else if (inputnum == 4) {
			changePassword(passwords, user);
		}
		else if (inputnum == 5) {
			return -1;
		}
		else if (inputnum == 0) {
			return 1;
		}
		system("PAUSE");
	}
	/* 내부 호출 함수: applyLecture, printStudent, printLectures, disapplyLecture, changePassword*/
}

void addStudent(vector<string>& studentIds, vector<string>& passwords, vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes) {
	string stuId, passwd, name;
	vector <string> k = {};
	int index;
	cout << "------------------------------------------------------------------------" << endl;
	cout << "학번: ";
	cin >> stuId;
	cout << "비밀번호: ";
	cin >> passwd;
	cout << "학생 이름: ";
	cin >> name;
	cout << "------------------------------------------------------------------------" << endl;
	index = findStudentById(studentIds, stuId);
	if (index != -1) {
		cout << "이미 존재하는 학번입니다." << endl;
	}
	else {
		studentIds.push_back(stuId);
		passwords.push_back(passwd);
		names.push_back(name);
		credits.push_back(18);
		appliedLectureCodes.push_back(k);
		cout << "성공적으로 등록되었습니다." << endl;
	}
}

void addLecture(vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits) {
	string lectureId, lectureName;
	int index, credit, limit;
	cout << "------------------------------------------------------------------------" << endl;
	cout << "과목코드 : ";
	cin >> lectureId;
	cout << "과목명 : ";
	cin >> lectureName;
	cout << "학점: ";
	cin >> credit;
	cout << "수강인원: ";
	cin >> limit;
	cout << "------------------------------------------------------------------------" << endl;

	index = findLectureByCode(lectureCodes, lectureId);
	if (index != -1) {
		cout << "이미 존재하는 과목코드입니다." << endl;
	}
	else {
		lectureCodes.push_back(lectureId);
		lectureNames.push_back(lectureName);
		lectureCredits.push_back(credit);
		limits.push_back(limit);
		cout << "성공적으로 강의가 개설되었습니다." << endl;
	}

}

void deleteLecture(vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits, vector<int>& credits, vector<vector<string>>& appliedLectureCodes) {
	string inputleco;
	int index;
	printLectures(appliedLectureCodes, lectureCodes, lectureNames, lectureCredits, limits, -100);
	while (true) {
		cout << "삭제할 강의 코드 (0: 뒤로가기) >> ";
		cin >> inputleco;

		if (inputleco == "0") {
			break;
		}

		index = findLectureByCode(lectureCodes, inputleco);

		if (index == -1) {
			cout << "일치하는 코드가 없습니다." << endl;
		}
		else {
			for (unsigned i = 0; i < appliedLectureCodes.size(); i++) {
				int elem = findLectureByCode(appliedLectureCodes[i], inputleco);
				if (elem != -1) {
					deleteElement(appliedLectureCodes[i], elem);
					credits[i] = credits[i] + lectureCredits[index];
				}
			}

			deleteElement(lectureCodes, index);
			deleteElement(lectureNames, index);
			lectureCredits.erase(lectureCredits.begin() + index);
			limits.erase(limits.begin() + index);
			cout << "성공적으로 강의가 폐쇄되었습니다." << endl;
		}
		system("PAUSE");
	}
}


int runAdmin(vector<string>& studentIds, vector<string>& passwords, vector<string>& names, vector<int>& credits, vector<vector<string>>& appliedLectureCodes, vector<string>& lectureCodes, vector<string>& lectureNames, vector<int>& lectureCredits, vector<int>& limits) {
	while (true) {
		int inputnum;
		cout << "------------------------------------------------------------------------" << endl;
		cout << "1.학생 추가" << endl;
		cout << "2.강의 개설" << endl;
		cout << "3.강의 삭제" << endl;
		cout << "4.로그아웃" << endl;
		cout << "0.종료" << endl;
		cout << "------------------------------------------------------------------------" << endl;
		cout << ">>";
		cin >> inputnum;

		if (inputnum == 1) {
			addStudent(studentIds, passwords, names, credits, appliedLectureCodes);
		}
		else if (inputnum == 2) {
			addLecture(lectureCodes, lectureNames, lectureCredits, limits);
		}
		else if (inputnum == 3) {
			deleteLecture(lectureCodes, lectureNames, lectureCredits, limits, credits, appliedLectureCodes);
		}
		else if (inputnum == 4) {
			return -1;
		}
		else if (inputnum == 0) {
			return 0;
		}
		system("PAUSE");
	}
}
