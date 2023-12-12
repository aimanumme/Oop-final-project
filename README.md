#include<iostream>
#include<Windows.h>
#include<fstream>
#include<string>
using namespace std;
class Filehandler
{
private :
public :
	//read
	void readFile(string filename)
	{
		ifstream file(filename);
		if (file.is_open())
		{
			string line;
			while (getline(file, line))
			{
				cout << line;
				cout << endl;
			}
			file.close();
		}
		else
		{
			cout << "Unable to open file: " << filename << " for reading" << endl;
		}
	}
	//write in file 
	void writeFile(string filename, string data)
	{
		ofstream write(filename, ios::app);
		if (write.is_open())
		{
			write << data;
			write.close();
		}
		else
		{
			cout << "Unable to open file: " << filename << " for writing<<endl";
		}
	}
	void enrollInCourse(string file)
	{
		string code, course, instructor; 
		string credits; 
		cout << "Enter the course code to enroll it : ";
		cin >> code;
		cout << endl << "Enter course name : ";
		cin >> course;
		cout << endl << "Enter the instructors name : ";
		cin >> instructor;
		cout << endl << "Enter the credit hours : ";
		cin >> credits;
		
		ofstream myfile("course.txt", ios::app);
		if (myfile.is_open())
		{
			myfile << code << ' ' << instructor << ' ' << credits << ' ' << course << ' ';
			myfile << endl;
			myfile.close();
			cout << "course enrolled successfully" << endl;
		}
		cout << "Course with code " << code << "enrolled in the courses successfully." << endl;
	}
	void removeCourse(const string& codeToRemove, const string& filename)
	{
		ifstream inFile(filename);
		ofstream temp("temp.txt");

		int found = 0;
		string code, instructor, credits, course;

		while (inFile >> code >> instructor >> credits >> course)
		{
			if (codeToRemove == code)
			{
				found = 1;
				cout << "Course with code " << codeToRemove << " deleted successfully" << endl;
			}
			else
			{
				temp << code << ' ' << instructor << ' ' << credits << ' ' << course << endl;
			}
		}

		inFile.close();
		temp.close();

		remove(filename.c_str());
		rename("temp.txt", filename.c_str());

		if (found == 0)
		{
			cout << "Course with code " << codeToRemove << " not found" << endl;
		}
	}

};
class Course
{
	void MakeAttendanceList()
	{
		for (int i = 0; i < 30; i++)
		{
			Attendance += "-";
		}
	}
	void MakeQuizList()
	{
		for (int i = 0; i < 5; i++)
		{
			quizmarks += "-";
		}
	}
	void MakeAssignmentList()
	{
		for (int i = 0; i < 5; i++)
		{
			assignmentmarks += "-";
		}
	}
public:
	string code;
	string name;
	string instructor;
	int credithour;
	string quizmarks;
	string assignmentmarks;
	int finalMarks;
	string Attendance;
	int totalAttendance;
	void ReadDataFromFile(ifstream& file)
	{
		string temp;
		file >> code;
		file >> name;
		file >> temp;
		instructor = temp;
		instructor += " ";
		file >> temp;
		instructor += temp;
		file >> credithour;
		MakeAttendanceList();
		totalAttendance = 0;
		MakeQuizList();
		MakeAssignmentList();
	}
	void Print()
	{
		cout << "Code: " << code << endl;
		cout << "Name: " << name << endl;
		cout << "Instructor: " << instructor << endl;
		cout << "Credithour: " << credithour << endl;
	}
	bool isMatch(string key)
	{
		return (key == name);
	}
	void Input()
	{
		string temp;
		cout << "Code: ";
		cin >> code;
		cout << "Name: ";
		cin >> name;
		cout << "Instructor First Name: ";
		cin >> temp;
		instructor = temp;
		instructor += " ";
		cout << "Instructor Last Name: ";
		cin >> temp;
		instructor += temp;
		cout << "Credit Hours: ";
		cin >> credithour;
		MakeAttendanceList();
		totalAttendance = 0;
		MakeQuizList();
		MakeAssignmentList();
	}
	void MarkAttendance()
	{
		int index = 0;
		cout << "Enter the date(1-30): ";
		cin >> index;
		index--;
		int choice;
		cout << "\n1.PRESENT\n2.ABSENT\n3.LATE\n4.UNMARK\n";
		cin >> choice;
		if (choice == 1)
		{
			if (Attendance[index] != 'P')
			{
				totalAttendance++;
				Attendance[index] = 'P';
			}
		}
		else if (choice == 2)
		{
			Attendance[index] = 'A';
		}
		else if (choice == 3)
		{
			Attendance[index] = 'L';
		}
		else
		{
			Attendance[index] = '-';
		}
	}
	void EvaluateQuizMark()
	{
		int index = 0;
		cout << "Enter the Number(1-5): ";
		cin >> index;
		index--;
		char choice;
		cout << "Enter the marks obtained(0-9): ";
		cin >> choice;
		quizmarks[index] = choice;
	}
	void EvaluateAssignmentMark()
	{
		int index = 0;
		cout << "Enter the Number(1-5): ";
		cin >> index;
		index--;
		char choice;
		cout << "Enter the marks obtained(0-9): ";
		cin >> choice;
		assignmentmarks[index] = choice;
	}
	void DisplayAttendance()
	{
		cout << "\n------------Attendance-------------------\n";
		for (int i = 0; i < 30; i++)
		{
			cout << i + 1 << "\t" << Attendance[i] << endl;
		}
	}
	void DisplayQuizMarks()
	{
		cout << "\n------------QUIZ-------------------\n";
		for (int i = 0; i < 5; i++)
		{
			cout << i + 1 << "\t" << quizmarks[i] << endl;
		}
	}
	void DisplayAssignmentMarks()
	{
		cout << "\n------------ASSIGNMENT-------------------\n";
		for (int i = 0; i < 5; i++)
		{
			cout << i + 1 << "\t" << assignmentmarks[i] << endl;
		}
	}
	void WriteDataInFile(ofstream& file)
	{
		file << "Code: " << code << endl;
		file << "Name: " << name << endl;
		file << "Instructor: " << instructor << endl;
		file << "Credithour: " << credithour << endl;
		file << "\n------------Attendance-------------------\n";
		for (int i = 0; i < 30; i++)
		{
			file << i + 1 << "\t" << Attendance[i] << endl;
		}
		file << "\n------------QUIZ-------------------\n";
		for (int i = 0; i < 5; i++)
		{
			file << i + 1 << "\t" << quizmarks[i] << endl;
		}
		file << "\n------------ASSIGNMENT-------------------\n";
		for (int i = 0; i < 5; i++)
		{
			file << i + 1 << "\t" << assignmentmarks[i] << endl;
		}
		file << "FINAL MARKS: " << finalMarks;
	}
	void SetFinalMarks(int marks)
	{
		finalMarks = marks;
		cout << "FINAL MARKS: " << finalMarks;
	}
};
class Student
{
	string fname;
	string lname;
	string rollNumber;
	string contact;
	int age;
	Course** courseList;
	int totalCourse;
public:
	Student(string roll)
	{
		rollNumber = roll;
	}
	Student()
	{
		totalCourse = 0;
	}
	void ReadDataFromFile(ifstream& file)
	{
		file >> fname;
		file >> lname;
		file >> rollNumber;
		file >> contact;
		file >> age;
	}
	void PrintCourse()
	{
		cout << "------------------CourseList----------------\n";
		for (int i = 0; i < totalCourse; i++)
		{
			cout << "----------------------------------\n";
			courseList[i]->Print();
		}
	}
	void Print()
	{
		cout << "Name: " << fname << " " << lname << endl;
		cout << "Roll Number: " << rollNumber << endl;
		cout << "Contact: " << contact << endl;
		cout << "Age: " << age << endl;
		PrintCourse();
	}
	bool isPresent(Student* ptr)
	{
		return (ptr->rollNumber == rollNumber);
	}
	bool isPresent(string key)
	{
		return (key == rollNumber);
	}
	void Input()
	{
		string temp;
		cout << "First Name: ";
		cin >> fname;
		cout << "Last Name: ";
		cin >> lname;
		cout << "\nRoll Number: ";
		cin >> rollNumber;
		cout << "\nContact: ";
		cin >> contact;
		cout << "\nAge: ";
		cin >> age;
	}
	void EditInfo()
	{
		string temp;
		int choice;
		cout << "1.FIRSTNAME\n2.LASTNAME\n3.ROLL NUMBER\n4.CONTACT\n5.AGE\n";
		cin >> choice;
		cout << "INPUT: ";
		cin >> temp;
		if (choice == 1)
		{
			fname = temp;
		}
		else if (choice == 2)
		{
			lname = temp;
		}
		else if (choice == 3)
		{
			rollNumber = temp;
		}
		else if (choice == 4)
		{
			contact = temp;
		}
		else if (choice == 5)
		{
			age = stoi(temp);
		}
	}
	void RegisterCourse(Course* ptr, const string& code)
	{
		if (SearchCourse(ptr->name))
		{
			cout << "Course is already registered\n";
			delete ptr;
			return;
		}
		ifstream coursesFile("course.txt");  // Assuming the courses file is named "courses.txt"
		string courseCodeFromFile;
		string courseNameFromFile;
		bool courseExists = false;

		// Check if the course code matches the provided code
		while (coursesFile >> courseCodeFromFile)
		{
			//// Read the rest of the course information (name) from the file
			//getline(coursesFile >> ws, courseNameFromFile);

			if (courseCodeFromFile == code)
			{
				courseExists = true;
				break;
			}
		}

		coursesFile.close();

		// If the course does not exist, inform the user
		if (!courseExists)
		{
			cout << "Invalid course code. The course does not exist in the courses file.\n";
			return;
		}

		if (courseExists)
		{
			Course** newList = new Course * [totalCourse + 1];
			for (int i = 0; i < totalCourse; i++)
			{
				newList[i] = courseList[i];
			}
			newList[totalCourse] = ptr;
			totalCourse++;
			courseList = newList;
			cout << "Course is registered successfully\n";
		}
		if (!courseExists)
		{
			cout << "Invalid course name. The course does not exist in the courses file.\n";
			return;
		}
		
	}
	void RemoveCourse(string name)
	{
		Course** newList = new Course * [totalCourse];
		int i = 0;
		int j = 0;
		while (j < totalCourse)
		{
			if (courseList[i]->name == name)
			{
				i++;
			}
			else
			{
				newList[j] = courseList[i];
				i++;
				j++;
			}
		}
		courseList = newList;
	}
	void WithDrawCourse(string name)
	{
		if (SearchCourse(name))
		{
			totalCourse--;
			RemoveCourse(name);
			cout << "Course withdrawl successful\n";
			return;
		}
		cout << "Course is not regsitered\n";
	}
	bool SearchCourse(string name)
	{
		for (int i = 0; i < totalCourse; i++)
		{
			if (courseList[i]->isMatch(name))
			{
				return true;
			}
		}
		return false;
	}
	Course* getCourse(string name)
	{
		for (int i = 0; i < totalCourse; i++)
		{
			if (courseList[i]->isMatch(name))
			{
				return courseList[i];
			}
		}
		return 0;
	}
	void MarkAttendance()
	{
		if (totalCourse == 0)
		{
			cout << "Student has not registered any course\n";
			return;
		}
		PrintCourse();
		string name;
		cout << "Enter the Course Name: ";
		cin >> name;
		Course* temp = getCourse(name);
		if (temp)
		{
			temp->DisplayAttendance();
			temp->MarkAttendance();
			temp->DisplayAttendance();
			return;
		}
		cout << "Student has not registered" << name << endl;
	}
	void EvaluateMarks()
	{
		if (totalCourse == 0)
		{
			cout << "Student has not registered any course\n";
			return;
		}
		PrintCourse();
		string name;
		cout << "Enter the Course Name: ";
		cin >> name;
		Course* temp = getCourse(name);
		if (temp)
		{
			int choice;
			cout << "1.QUIZ\n2.ASSIGNMENTS\n3.FINAL\n4.EXIT\n";
			cin >> choice;
			if (choice == 1)
			{
				temp->DisplayQuizMarks();
				temp->EvaluateQuizMark();
				temp->DisplayQuizMarks();
				return;
			}
			if (choice == 2)
			{
				temp->DisplayAssignmentMarks();
				temp->EvaluateAssignmentMark();
				temp->DisplayAssignmentMarks();
				return;
			}
			if (choice == 3)
			{
				int marks;
				cout << "Enter the marks obtained: ";
				cin >> marks;
				temp->SetFinalMarks(marks);
				return;
			}
			if (choice == 4)
			{
				cout << "BYE BYE!!!!\n";
				return;
			}
		}
		cout << "Student has not registered" << name << endl;
	}
	void PrintAttendance()
	{
		if (totalCourse == 0)
		{
			cout << "Student has not registered any course\n";
			return;
		}
		PrintCourse();
		string name;
		cout << "Enter the Course Name: ";
		cin >> name;
		Course* temp = getCourse(name);
		if (temp)
		{
			temp->DisplayAttendance();
			return;
		}
		cout << "Student has not registered" << name << endl;
	}
	void WriteDataInFile(ofstream& file)
	{
		file << "FIRST NAME: " << fname << endl;
		file << "LAST NAME: " << lname << endl;
		file << "ROLL NUMBER: " << rollNumber << endl;
		file << "CONTACT#: " << contact << endl;
		file << "AGE: " << age << endl;
		for (int i = 0; i < totalCourse; i++)
		{
			courseList[i]->WriteDataInFile(file);
		}
	}
};
class Flex
{
	Student** studentList;
	int totalStudents;
	Course** courseList;
	int totalCourses;
public:
	Flex()
	{
		totalStudents = totalCourses = 0;
		studentList = 0;
		courseList = 0;
	}
	void CalculateTotalstudents()
	{
		ifstream file;
		string line;
		file.open("students.txt");
		while (getline(file, line))
		{
			totalStudents++;
		}
	}
	void CalculateTotalCources()
	{
		ifstream file;
		string line;
		file.open("course.txt");
		while (getline(file, line))
		{
			totalCourses++;
		}
	}
	void ReadCourcesFromFile()
	{
		CalculateTotalCources();
		ifstream file;
		file.open("course.txt");
		courseList = new Course * [totalCourses];
		for (int i = 0; i < totalCourses; i++)
		{
			courseList[i] = new Course;
			courseList[i]->ReadDataFromFile(file);
		}
	}
	void ReadStudentsFromFile()
	{
		CalculateTotalstudents();
		ifstream file;
		file.open("student.txt");
		studentList = new Student * [totalStudents];
		for (int i = 0; i < totalStudents; i++)
		{
			studentList[i] = new Student;
			studentList[i]->ReadDataFromFile(file);
		}
	}
	Student* getStudent(string rollNumber)
	{
		for (int i = 0; i < totalStudents; i++)
		{
			if (studentList[i]->isPresent(rollNumber))
			{
				return studentList[i];
			}
		}
		return 0;
	}
	bool isPresent(Student* key)
	{
		for (int i = 0; i < totalStudents; i++)
		{
			if (studentList[i]->isPresent(key))
			{
				return true;
			}
		}
		return false;
	}
	void AddStudentInList(Student* ptr)
	{
		Student** newList = new Student * [totalStudents + 1];
		for (int i = 0; i < totalStudents; i++)
		{
			newList[i] = studentList[i];
		}
		newList[totalStudents] = ptr;
		totalStudents++;
		delete[] studentList;
		studentList = newList;
	}
	void EnrollStudent()
	{
		Student* newStudent = new Student;
		newStudent->Input();
		if (isPresent(newStudent))
		{
			cout << "Student Already enrolled\n";
			delete newStudent;
			return;
		}
		AddStudentInList(newStudent);
		cout << "Student added successfully\n";
	}
	void RemoveSStudent(Student* ptr)
	{
		Student** newList = new Student * [totalStudents];
		int i = 0;
		int j = 0;
		while (j < totalStudents)
		{
			if (studentList[i]->isPresent(ptr))
			{
				i++;
			}
			else
			{
				newList[j] = studentList[i];
				i++;
				j++;
			}
		}
		delete[] studentList;
		studentList = newList;
	}
	void RemoveStudent()
	{
		string rollNumber;
		cout << "Enter the RollNumber of Student(e.g 21L-1234): ";
		cin >> rollNumber;
		Student* newStudent = new Student(rollNumber);
		if (isPresent(newStudent))
		{
			totalStudents--;
			RemoveSStudent(newStudent);
			cout << "Student Removed Succesfully\n";
			return;
		}
		cout << "Student is Not Enrolled\n";
		delete newStudent;
	}
	void PrintStudent()
	{
		cout << "------------------StudentList----------------\n";
		for (int i = 0; i < totalStudents; i++)
		{
			cout << "----------------------------------\n";
			studentList[i]->Print();
		}
	}
	void PrintCourse()
	{
		cout << "------------------CourseList----------------\n";
		for (int i = 0; i < totalCourses; i++)
		{
			cout << "----------------------------------\n";
			courseList[i]->Print();
		}
	}
	void Print()
	{
		PrintStudent();
		cout << endl;
		PrintCourse();
	}
	bool Authentication()
	{
		string domain;
		cout << "Enter your domain: ";
		cin >> domain;
		cout << "Please wait...\n";
		Sleep(1000);
		if (domain == "NU" || domain == "FAST" || domain == "nu")
		{
			cout << "AUTHENTICATION PROVIDED!!!!\n";
			return true;
		}
		cout << "AUTHENTICATION FAILED!!!!\n";
		return false;
	}
	void CourseRegistration()
	{
		PrintCourse(); 
		string rollNumber;
		cout << "Enter the RollNumber of Student(e.g 21L-1234): ";
		cin >> rollNumber;
		Student* newStudent = getStudent(rollNumber);
		if (newStudent)
		{
			int choice;
			cout << "1.REGISTER COURSE.\n2.WITHDRAW COURSE.\n3.EXIT.\n";
			cin >> choice;
			if (choice == 1)
			{
				string y; 
				Course* newCourse = new Course;
				newCourse->Input();
				cout << "\n Please Enter code again for verification" << endl;
				cin >> y; 
				newStudent->RegisterCourse(newCourse, y);
				return;
			}
			if (choice == 2)
			{
				string name;
				newStudent->PrintCourse();
				cout << "Enter the Course Name: ";
				cin >> name;
				newStudent->WithDrawCourse(name);
				return;
			}
			else
			{
				return;
			}
		}
		cout << "Student is not enrolled\n";
	}
	void MarkAttendance()
	{
		PrintStudent();
		string rollNumber;
		cout << "Enter the RollNumber of Student(e.g 21L-1234): ";
		cin >> rollNumber;
		Student* newStudent = getStudent(rollNumber);
		if (newStudent)
		{
			newStudent->MarkAttendance();
			return;
		}
		cout << "Student is not enrolled\n";
	}
	void EvaluateMarks()
	{
		PrintStudent();
		string rollNumber;
		cout << "Enter the RollNumber of Student(e.g 21L-1234): ";
		cin >> rollNumber;
		Student* newStudent = getStudent(rollNumber);
		if (newStudent)
		{
			newStudent->EvaluateMarks();
			return;
		}
		cout << "Student is not enrolled\n";
	}
	void EditStudent()
	{
		PrintStudent();
		string rollNumber;
		cout << "Enter the RollNumber of Student(e.g 21L-1234): ";
		cin >> rollNumber;
		/*Student b;
		b.EditInfo(); */
		Student* newStudent = getStudent(rollNumber);
		if (newStudent)
		{
			newStudent->EditInfo();
			return;
		}
		cout << "Student is not enrolled\n";
	}
	void DisplayMenu()
	{
		int press;
		do
		{
			cout << "\t\t\t\t\t\t\tMAIN MENU " << endl;
			cout << endl << "1 - Enroll a student" << endl << "2 - Course Registration" << endl << "3 - Attendance " << endl << "4 - Marks" << endl << "5 - Exit" << endl;
			cout << "Enter your choice ";

			cin >> press;
			if (press == 1)
			{
				int choice;
				do
				{
					cout << endl << "1.Display already enrolled students" << endl << "2.Add a student" << endl << "3.Remove a student" << endl << "4.Edit student detail" << endl << "5.back" << endl;
					cin >> choice;
					if (choice == 1)
					{
						PrintStudent();
					}
					else if (choice == 2)
					{
						EnrollStudent();
					}
					else if (choice == 3)
					{
						RemoveStudent();
					}
					else if (choice == 4)
					{
						EditStudent();
					}
					else if (choice == 5)
					{
						cout << "BYE BYE!!!!\n";
					}
					else
					{
						cout << "Please enter accurately " << endl; 
					}
				} while (choice != 5);

			}
			else if (press == 2)
			{
				char a; 
				cout << "Do you want to Register Student in course? Press y for yes and n for no" << endl;
				cin >> a; 
				if (a == 'y')
				{
					CourseRegistration();
				}
				else if (a == 'n')
				{
					int f;
					string e;
					cout << "1.Add Course\n2.Drop Course" << endl;
					cin >> f; 
					Filehandler s;
					if (f == 1)
					{
						string a = "course.txt"; 
						s.enrollInCourse("course.txt");
						cout << endl;
						Filehandler t; 
						t.readFile(a); 
					}
					else if (f == 2)
					{
						Filehandler f; 
						cout << "Please enter course code: " << endl;
						cin >> e;
						s.removeCourse(e, "course.txt");
						cout << endl; 
						f.readFile("course.txt"); 
					}   
					
				}
			
			}
			else if (press == 3)
			{
				MarkAttendance();
			}
			else if (press == 4)
			{
				EvaluateMarks();
			}
			else if (press == 5)
			{
				UpdateFile();
				cout << "Exiting the program" << endl;
			}
			else
			{
				cout << "Please Enter Accurately " << endl; 
				break; 
			}

		} while (press != 5);

	}
	void UpdateFile()
	{
		ofstream file;
		file.open("student.txt");
		for (int i = 0; i < totalStudents; i++)
		{
			studentList[i]->WriteDataInFile(file);
		}
	}
};

void main()
{
	Flex obj;
	if (obj.Authentication())
	{
		cout << "         Welcome\n";
		cout << "Fetching Data";
		for (int i = 0; i < 5; i++)
		{
			cout << ". ";
			Sleep(1000);
		}
		system("cls");
	Flex obj; 
		obj.ReadCourcesFromFile(); 
		cout << "         FLEX PORTAL\n";
		obj.DisplayMenu();
	}
}
