import express from "express";
import cors from "cors";

const app = express();
app.use(cors());
app.use(express.json());

// Mock course data
const courses = {
  "course-101": {
    id: "course-101",
    name: "Web Fundamentals",
    price: 49,
    description: "Learn HTML, CSS, JS from scratch."
  },
  "course-102": {
    id: "course-102",
    name: "React Basics",
    price: 79,
    description: "Introduction to React components & hooks."
  }
};

// 📌 POST /api/enroll
app.post("/api/enroll", (req, res) => {
  const { userId, courseId } = req.body;

  if (!userId || !courseId) {
    return res.status(400).json({ message: "Missing userId or courseId" });
  }

  return res.json({
    message: "Enrollment successful!",
    enrollmentId: "ENR-" + Math.floor(Math.random() * 99999),
    courseId,
  });
});

// 📌 POST /api/payment
app.post("/api/payment", (req, res) => {
  const { userId, courseId, method } = req.body;

  if (!method) {
    return res.status(400).json({ message: "Missing payment method" });
  }

  return res.json({
    message: "Payment processed",
    status: "success",
    method
  });
});

// 📌 POST /api/enroll/confirmation
app.post("/api/enroll/confirmation", (req, res) => {
  const { enrollmentId, email } = req.body;

  return res.json({
    message: `Confirmation email sent to ${email}`,
    enrollmentId
  });
});

app.get("/api/course/:courseId", (req, res) => {
  const course = courses[req.params.courseId];
  if (!course) {
    return res.status(404).json({ message: "Course not found" });
  }
  return res.json(course);
});

app.listen(5000, () => console.log("Server running at http://localhost:5000"));
