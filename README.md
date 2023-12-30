Question 

Find the number of users who are absent and task is not submitted between 15 oct-2020 and 31-oct-2020


```javascript
db.users.aggregate([
  {
    $lookup: {
      from: "attendance",
      localField: "user_Attendance",
      foreignField: "_id",
      as: "user_Attendance",
    },
  },
  {
    $lookup: {
      from: "tasks",
      localField: "user_SubmittedTasks",
      foreignField: "_id",
      as: "user_SubmittedTasks",
    },
  },

  {
    $project: {
      user_Name: 1,
      user_Email: 1,
      user_Phone: 1,
      user_Batch: 1,
      user_Attendance: { classDate: 1, classType: 1 },
      user_SubmittedTasks: {
        task_Name: 1,
        task_Date: 1,
        task_submittedDate: 1,
      },
    },
  },
  {
    $match: {
      "user_Attendance.classDate": {
        $gt: ISODate("2020-10-15"),
        $lt: ISODate("2020-10-30"),
      },
    },
  },
]);
```
