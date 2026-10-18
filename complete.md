<?php require_once 'core/dbConfig.php'; 


$edit_id = '';
$firstname = '';
$lastname = '';
$gender = '';
$email = '';
$college = '';
$date_added = date('Y-m-d');
$position_applied_for = '';

// --- 1. HANDLE INSERT & UPDATE ---
if (isset($_POST['save_applicant'])) {
    $applicant_id = $_POST['applicant_id'];
    $firstname = $_POST['firstname'];
    $lastname = $_POST['lastname'];
    $gender = $_POST['gender'];
    $email = $_POST['email'];
    $college = $_POST['college'];
    $date_added = $_POST['date_added'];
    $position_applied_for = $_POST['position_applied_for'];

    if (!empty($_POST['is_update'])) {
        // UPDATE existing record
        $stmt = $pdo->prepare("UPDATE job_application SET 
            firstname = ?, lastname = ?, gender = ?, email = ?, college = ?, date_added = ?, position_applied_for = ? 
            WHERE applicant_id = ?");
        $stmt->execute([$firstname, $lastname, $gender, $email, $college, $date_added, $position_applied_for, $applicant_id]);
    } else {
        // INSERT new record
        $stmt = $pdo->prepare("INSERT INTO job_application 
            (applicant_id, firstname, lastname, gender, email, college, date_added, position_applied_for) 
            VALUES (?, ?, ?, ?, ?, ?, ?, ?)");
        $stmt->execute([$applicant_id, $firstname, $lastname, $gender, $email, $college, $date_added, $position_applied_for]);
    }
    header("Location: index.php");
    exit();
}

// --- 2. HANDLE DELETE ---
if (isset($_GET['delete'])) {
    $id = $_GET['delete'];
    $stmt = $pdo->prepare("DELETE FROM job_application WHERE applicant_id = ?");
    $stmt->execute([$id]);
    header("Location: index.php");
    exit();
}

// --- 3. HANDLE LOAD DATA FOR EDITING ---
if (isset($_GET['edit'])) {
    $id = $_GET['edit'];
    $stmt = $pdo->prepare("SELECT * FROM job_application WHERE applicant_id = ?");
    $stmt->execute([$id]);
    $row = $stmt->fetch(PDO::FETCH_ASSOC);

    if ($row) {
        $edit_id = $row['applicant_id'];
        $firstname = $row['firstname'];
        $lastname = $row['lastname'];
        $gender = $row['gender'];
        $email = $row['email'];
        $college = $row['college'];
        $date_added = $row['date_added'];
        $position_applied_for = $row['position_applied_for'];
    }
}

// --- 4. FETCH ALL RECORDS FOR THE TABLE ---
$stmt = $pdo->query("SELECT * FROM job_application ORDER BY applicant_id DESC");
$applications = $stmt->fetchAll(PDO::FETCH_ASSOC);
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Job Application System - Act2Taylo</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 30px; background-color: #f8f9fa; }
        .container { max-width: 1000px; margin: 0 auto; background: #fff; padding: 25px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        h2, h3 { color: #333; }
        .form-group { margin-bottom: 12px; }
        label { display: inline-block; width: 170px; font-weight: bold; color: #555; }
        input[type="text"], input[type="email"], input[type="date"], select { width: 280px; padding: 6px 10px; border: 1px solid #ccc; border-radius: 4px; }
        .btn { padding: 8px 16px; cursor: pointer; border: none; border-radius: 4px; text-decoration: none; display: inline-block; font-size: 14px; }
        .btn-submit { background-color: #28a745; color: white; }
        .btn-cancel { background-color: #6c757d; color: white; margin-left: 10px; }
        .btn-edit { background-color: #007bff; color: white; padding: 4px 8px; font-size: 12px; }
        .btn-delete { background-color: #dc3545; color: white; padding: 4px 8px; font-size: 12px; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 10px; text-align: left; }
        th { background-color: #007bff; color: white; }
        tr:nth-child(even) { background-color: #f2f2f2; }
    </style>
</head>
<body>

<div class="container">
    <h2>Job Application System</h2>

    <!-- FORM WITH 8 INPUTS -->
    <form method="POST" action="index.php">
        <input type="hidden" name="is_update" value="<?php echo $edit_id ? '1' : ''; ?>">

        <div class="form-group">
            <label for="applicant_id">1. Applicant ID:</label>
            <input type="text" id="applicant_id" name="applicant_id" value="<?php echo htmlspecialchars($edit_id); ?>" <?php echo $edit_id ? 'readonly' : 'required'; ?>>
        </div>

        <div class="form-group">
            <label for="firstname">2. First Name:</label>
            <input type="text" id="firstname" name="firstname" value="<?php echo htmlspecialchars($firstname); ?>" required>
        </div>

        <div class="form-group">
            <label for="lastname">3. Last Name:</label>
            <input type="text" id="lastname" name="lastname" value="<?php echo htmlspecialchars($lastname); ?>" required>
        </div>

        <div class="form-group">
            <label for="gender">4. Gender:</label>
            <select id="gender" name="gender" required>
                <option value="">--Select--</option>
                <option value="Male" <?php if($gender == 'Male') echo 'selected'; ?>>Male</option>
                <option value="Female" <?php if($gender == 'Female') echo 'selected'; ?>>Female</option>
            </select>
        </div>

        <div class="form-group">
            <label for="email">5. Email:</label>
            <input type="email" id="email" name="email" value="<?php echo htmlspecialchars($email); ?>" required>
        </div>

        <div class="form-group">
            <label for="college">6. College/University:</label>
            <input type="text" id="college" name="college" value="<?php echo htmlspecialchars($college); ?>" required>
        </div>

        <div class="form-group">
            <label for="date_added">7. Date Added:</label>
            <input type="date" id="date_added" name="date_added" value="<?php echo htmlspecialchars($date_added); ?>" required>
        </div>

        <div class="form-group">
            <label for="position_applied_for">8. Position Applied For:</label>
            <input type="text" id="position_applied_for" name="position_applied_for" value="<?php echo htmlspecialchars($position_applied_for); ?>" required>
        </div>

        <div class="form-group" style="margin-top: 15px;">
            <button type="submit" name="save_applicant" class="btn btn-submit">
                <?php echo $edit_id ? 'Update Record' : 'Add Applicant'; ?>
            </button>
            <?php if ($edit_id): ?>
                <a href="index.php" class="btn btn-cancel">Cancel Edit</a>
            <?php endif; ?>
        </div>
    </form>

    <hr style="margin: 30px 0;">

    <!-- DATABASE DISPLAY TABLE -->
    <h3>Application Records</h3>
    <table>
        <thead>
            <tr>
                <th>ID</th>
                <th>First Name</th>
                <th>Last Name</th>
                <th>Gender</th>
                <th>Email</th>
                <th>College</th>
                <th>Date Added</th>
                <th>Position</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <?php if (count($applications) > 0): ?>
                <?php foreach ($applications as $row): ?>
                    <tr>
                        <td><?php echo htmlspecialchars($row['applicant_id']); ?></td>
                        <td><?php echo htmlspecialchars($row['firstname']); ?></td>
                        <td><?php echo htmlspecialchars($row['lastname']); ?></td>
                        <td><?php echo htmlspecialchars($row['gender']); ?></td>
                        <td><?php echo htmlspecialchars($row['email']); ?></td>
                        <td><?php echo htmlspecialchars($row['college']); ?></td>
                        <td><?php echo htmlspecialchars($row['date_added']); ?></td>
                        <td><?php echo htmlspecialchars($row['position_applied_for']); ?></td>
                        <td>
                            <a href="index.php?edit=<?php echo $row['applicant_id']; ?>" class="btn btn-edit">Edit</a>
                            <a href="index.php?delete=<?php echo $row['applicant_id']; ?>" class="btn btn-delete" onclick="return confirm('Are you sure you want to delete this applicant?');">Delete</a>
                        </td>
                    </tr>
                <?php endforeach; ?>
            <?php else: ?>
                <tr>
                    <td colspan="9" style="text-align: center;">No records found in database.</td>
                </tr>
            <?php endif; ?>
        </tbody>
    </table>
</div>

</body>
</html>
