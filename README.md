# Contact-Us_-form
<!DOCTYPE html>
<html>
<head>
  <title>Contact Us Form</title>
  <style>
    /* Styles remain the same */
  </style>
</head>
<body>
  <h2>Contact Us</h2>
  <div class="container">
    <?php
      $name = $email = $message = '';
      $errors = [];
      $successMessage = '';

      if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        // Validate name
        $name = test_input($_POST['name']);
        if (empty($name)) {
          $errors[] = 'Name is required';
        }

        // Validate email
        $email = test_input($_POST['email']);
        if (empty($email)) {
          $errors[] = 'Email is required';
        } elseif (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
          $errors[] = 'Invalid email format';
        }

        // Validate message
        $message = test_input($_POST['message']);
        if (empty($message)) {
          $errors[] = 'Message is required';
        }

        // If there are no errors, send the email
        if (empty($errors)) {
          $to = 'your-email@example.com';
          $subject = 'New Contact Us Message';
          $body = "Name: $name\nEmail: $email\nMessage: $message";

          if (mail($to, $subject, $body)) {
            $successMessage = 'Thank you for contacting us. We will get back to you soon.';
          } else {
            $errors[] = 'Oops! An error occurred while sending your message.';
          }
        }
      }

      // Function to sanitize form input
      function test_input($data) {
        $data = trim($data);
        $data = stripslashes($data);
        $data = htmlspecialchars($data);
        return $data;
      }
    ?>

    <?php if (!empty($successMessage)) : ?>
      <p class="success"><?php echo $successMessage; ?></p>
    <?php endif; ?>

    <?php if (!empty($errors)) : ?>
      <ul class="error">
        <?php foreach ($errors as $error) : ?>
          <li><?php echo $error; ?></li>
        <?php endforeach; ?>
      </ul>
    <?php endif; ?>

    <form method="POST" action="<?php echo htmlspecialchars($_SERVER['PHP_SELF']); ?>">
      <div class="form-group">
        <label for="name">Name</label>
        <input type="text" id="name" name="name" value="<?php echo htmlspecialchars($name); ?>">
      </div>

      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" name="email" value="<?php echo htmlspecialchars($email); ?>">
      </div>

      <div class="form-group">
        <label for="message">Message</label>
        <textarea id="message" name="message"><?php echo htmlspecialchars($message); ?></textarea>
      </div>

      <div class="form-group">
        <input type="submit" value="Submit">
      </div>
    </form>
  </div>
</body>
</html>

