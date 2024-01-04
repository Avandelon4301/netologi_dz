class Student:
  def __init__(self, name, surname, gender):
      self.name = name
      self.surname = surname
      self.gender = gender
      self.finished_courses = []
      self.courses_in_progress = []
      self.grades = {}

  def rate_lecture(self, lecturer, course, grade):
      if isinstance(lecturer, Lecturer) and course in self.courses_in_progress:
          if course in lecturer.lecture_grades:
              lecturer.lecture_grades[course].append(grade)
          else:
              lecturer.lecture_grades[course] = [grade]
      else:
          return 'ошибка'

  def calculate_avg_grade(self):
      total_grades = 0
      total_count = 0
      for grades in self.grades.values():
          total_grades += sum(grades)
          total_count += len(grades)
      if total_count != 0:
          return round(total_grades / total_count, 1)
      else:
          return 0

  def __str__(self):
      return f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за домашние задания: {self.calculate_avg_grade()}\nКурсы в процессе изучения: {", ".join(self.courses_in_progress)}\nЗавершенные курсы: {", ".join(self.finished_courses)}'

class Mentor:
  def __init__(self, name, surname):
      self.name = name
      self.surname = surname
      self.courses_attached = []

  def __str__(self):
      return f'Имя: {self.name}\nФамилия: {self.surname}'

class Lecturer(Mentor):
  def __init__(self, name, surname):
      super().__init__(name, surname)
      self.lecture_grades = {}

  def calculate_avg_grade(self):
      total_grades = 0
      total_count = 0
      for grades in self.lecture_grades.values():
          total_grades += sum(grades)
          total_count += len(grades)
      if total_count != 0:
          return round(total_grades / total_count, 1)
      else:
          return 0

  def __str__(self):
      avg_grade = self.calculate_avg_grade()
      return f'{super().__str__()}\nСредняя оценка за лекции: {avg_grade}'

  def __lt__(self, other):
      return self.calculate_avg_grade() < other.calculate_avg_grade()

  def __gt__(self, other):
      return self.calculate_avg_grade() > other.calculate_avg_grade()

class Reviewer(Mentor):

  def __str__(self):
    return f'Имя: {self.name}\nФамилия: {self.surname}'
    
  def rate_hw(self, student, course, grade):
      if isinstance(student, Student) and course in self.courses_attached:
          if course in student.grades:
              student.grades[course].append(grade)
          else:
              student.grades[course] = [grade]
      else:
          return 'ошибка'

def calculate_avg_grade_for_course(students, course):
  total_grades = 0
  total_count = 0
  for student in students:
      if course in student.grades:
          total_grades += sum(student.grades[course])
          total_count += len(student.grades[course])
  if total_count != 0:
      return round(total_grades / total_count, 1)
  else:
      return 0

def calculate_avg_grade_for_lectures(lecturers, course):
  total_grades = 0
  total_count = 0
  for lecturer in lecturers:
      if course in lecturer.lecture_grades:
          total_grades += sum(lecturer.lecture_grades[course])
          total_count += len(lecturer.lecture_grades[course])
  if total_count != 0:
      return round(total_grades / total_count, 1)
  else:
      return 0

best_student = Student('ruoy', 'eman', 'your_gender')
best_student.courses_in_progress += ['python', 'git']
best_student.finished_courses += ['Введение в программирование']

cool_mentor = Reviewer('some', 'buddy')
cool_mentor.courses_attached += ['python', 'git']

cool_mentor.rate_hw(best_student, 'python', 9)
cool_mentor.rate_hw(best_student, 'python', 10)
cool_mentor.rate_hw(best_student, 'python', 10)

some_lecturer = Lecturer('some', 'buddy')
some_lecturer.courses_attached += ['python', 'git']

best_student.rate_lecture(some_lecturer, 'python', 8)
best_student.rate_lecture(some_lecturer, 'python', 9)
best_student.rate_lecture(some_lecturer, 'python', 10)

print(cool_mentor)
print()
print(some_lecturer)
print()
print(best_student)
print()

print(f'Средняя оценка за домашние задания по курсу "python": {calculate_avg_grade_for_course([best_student], "python")}')
print(f'Средняя оценка за лекции по курсу "python": {calculate_avg_grade_for_lectures([some_lecturer], "python")}')