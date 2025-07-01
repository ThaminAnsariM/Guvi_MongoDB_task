# Guvi_MongoDB_task

1) Find all the topics and tasks which are thought in the month of October

db.topics.find({
  date: {
    $gte: ISODate("2020-10-01T00:00:00Z"),
    $lte: ISODate("2020-10-31T23:59:59Z")
  }
})

2) Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020

db.Company_drives.find({
  date: {
    $gte: "2020-10-15T00:00:00Z",
    $lte: "2020-10-31T23:59:59Z"
  }
})



3.Find all the company drives and students who are appeared for the placement.


db.company_drives.find(
  { students_appeared: { $exists: true, $ne: [] } },
  { company: 1, date: 1, students_appeared: 1, _id: 0 }
)

4. Find the number of problems solved by each user in CodeKata

db.codeKata.aggregate([
  {
    $group: {
      _id: "$user_id",
      total_solved: { $sum: "$problems_solved" }
    }
  }
])

5.Find all the mentors who have more than 15 mentees

db.mentors.find({
  mentees: { $exists: true, $type: "array" },
  $expr: {
    $gt: [{ $size: "$mentees" }, 15]
  }
})


6.Find the number of users who were absent and didn't submit the task between 15 Oct and 31 Oct 2020

const absentUsers = db.attendance.find({
  date: {
    $gte: "2020-10-15",
    $lte: "2020-10-31"
  },
  status: "absent"
}).toArray().map(doc => doc.user_id);


const submittedUsers = db.task_submissions.find({
  user_id: { $in: absentUsers },
  date: {
    $gte: "2020-10-15",
    $lte: "2020-10-31"
  }
}).toArray().map(doc => doc.user_id);


const notSubmitted = absentUsers.filter(id => !submittedUsers.includes(id));

print("Users absent and didn't submit task:", notSubmitted.length);


