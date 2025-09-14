---
title: Свържете се
description: Контакт

---

<p>
  <strong>Име:</strong> Пламка Илиева<br>
  <strong>Телефон:</strong> <a href="tel:+359898537438">+359 89 853 7438</a><br>
  <strong>Имейл:</strong> <a href="mailto:pl.ilieva@abv.bg">pl.ilieva@abv.bg</a>
</p>


<hr>

<form name="contact" method="POST" data-netlify="true" onsubmit="return handleSubmit(event);">
  <input type="hidden" name="form-name" value="contact" />
  <p>
    <label for="name">Имена:</label><br>
    <input type="text" id="name" name="name" required />
  </p>
  <p>
    <label for="email">Имейл:</label><br>
    <input type="email" id="email" name="email" required />
  </p>
  <p>
    <label for="message">Запитване:</label><br>
    <textarea id="message" name="message" rows="5" required></textarea>
  </p>
  <p>
    <button type="submit" style="background-color: #004aad">Изпрати</button>
  </p>
</form>

<div id="success-message" style="display: none; color: green;">
  <h3>Вашето запитване беше успешно изпратено!</h3>
</div>

<script>
  function handleSubmit(event) {
    event.preventDefault(); // Prevent the default form submission
    const form = event.target;
    const successMessage = document.getElementById("success-message");

    fetch("/", {
      method: "POST",
      body: new FormData(form),
    })
      .then(() => {
        form.style.display = "none"; // Hide the form
        successMessage.style.display = "block"; // Show success message
      })
      .catch((error) => {
        console.error("Form submission error:", error);
      });
  }
</script>
