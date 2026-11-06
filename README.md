# student-management-system
class Student {
  constructor(id, name, grades, attendance) {
    this.id = id;
    this.name = name;
    this.grades = grades;
    this.attendance = attendance;
  }
  calculateAverage() {
    return this.grades.reduce((a, b) => a + b, 0) / this.grades.length;
  }
  getGradeLetter(avg) {
    if (avg >= 90) return 'A';
    if (avg >= 80) return 'B';
    if (avg >= 70) return 'C';
    if (avg >= 60) return 'D';
    if (avg >= 50) return 'E';
    return 'F';
  }
  getFinalStatus(avg) {
    return avg >= 50 && this.attendance >= 75 ? 'ناجح' : 'يحتاج مراجعة';
  }
}

const studentsList = [
  new Student(1, 'أحمد محمد', [85, 90, 88], 85),
  new Student(2, 'سارة علي', [42, 45, 48], 80),
  new Student(3, 'محمود خالد', [60, 65, 70], 70),
  new Student(4, 'فاطمة عمر', [44, 46, 40], 90)
];

const strugglingStudents = studentsList.filter(s => {
  const avg = s.calculateAverage();
  return avg >= 40 && avg < 50;
});

const adjustedStruggling = strugglingStudents.map(s => {
  const oldAvg = s.calculateAverage();
  const newAvg = oldAvg + 5;
  return {
    name: s.name,
    oldAverage: oldAvg,
    newAverage: newAvg,
    status: newAvg >= 50 ? 'ناجح بعد الرأفة' : 'راسب'
  };
});

const totalNewAverages = adjustedStruggling.reduce((sum, s) => sum + s.newAverage, 0);
const averageAfterGrace = adjustedStruggling.length > 0 ? totalNewAverages / adjustedStruggling.length : 0;

console.log('--- تقرير أداء الطلاب بالكامل ---');
studentsList.forEach(s => {
  const avg = s.calculateAverage();
  console.log(`الاسم: ${s.name} | المتوسط: ${avg.toFixed(2)} | التقدير: ${s.getGradeLetter(avg)} | النتيجة: ${s.getFinalStatus(avg)}`);
});

console.log('\n--- قائمة الطلاب المتعثرين بعد تطبيق الرأفة ---');
console.table(adjustedStruggling);
console.log(`\nمتوسط درجات الطلاب المتعثرين بعد التعديل: ${averageAfterGrace.toFixed(2)}`);
