<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ADIS</title>
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
    />
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: "Segoe UI", Arial, sans-serif;
      }

      body {
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        background-image: url("https://i.pinimg.com/736x/bc/71/da/bc71da46862292caed1969114bd1cd92.jpg");
        background-size: cover; /* Fix 1: Ensure image covers entire viewport */
        background-position: center; /* Fix 2: Center the image */
        background-repeat: no-repeat; /* Fix 3: Prevent tiling */
        background-attachment: fixed; /* Optional: Keep background fixed during scroll */
        padding: 20px;
        margin: 0;
      }

      .login-container {
        background-color: rgb(254, 254, 254);
        border-radius: 8px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        width: 100%;
        max-width: 420px;
        padding: 30px;
        border-top: 4px solid #2563eb;
      }

      .login-header {
        text-align: center;
        margin-bottom: 25px;
        padding-bottom: 15px;
        border-bottom: 1px solid #e5e7eb;
      }

      .login-header h1 {
        color: #000000;
        font-weight: 600;
        font-size: 24px;
        margin-bottom: 5px;
      }

      .login-header p {
        color: #64748b;
        font-size: 14px;
      }

      .input-group {
        margin-bottom: 18px;
      }

      .input-group label {
        display: block;
        margin-bottom: 6px;
        color: #374151;
        font-weight: 500;
        font-size: 14px;
      }

      .input-group input,
      .input-group select {
        width: 100%;
        padding: 10px 12px;
        border: 1px solid #d1d5db;
        border-radius: 4px;
        font-size: 14px;
        transition: all 0.2s;
        color: #1f2937;
      }

      .input-group input:focus,
      .input-group select:focus {
        border-color: #2563eb;
        outline: none;
        box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.15);
      }

      .password-input {
        position: relative;
      }

      .toggle-password {
        position: absolute;
        right: 12px;
        top: 50%;
        transform: translateY(-50%);
        background: none;
        border: none;
        color: #6b7280;
        cursor: pointer;
        font-size: 16px;
      }

      .captcha-container {
        margin-top: 15px;
        padding: 12px;
        background: #f8fafc;
        border: 1px solid #e2e8f0;
        border-radius: 6px;
        display: none;
      }

      .captcha-row {
        display: flex;
        align-items: center;
        gap: 8px;
      }

      .captcha-display {
        font-size: 16px; /* Reduced from 20px */
        font-weight: 600;
        letter-spacing: 1px; /* Reduced from 2px */
        color: #1e293b;
        background: #f1f5f9;
        padding: 6px 10px; /* Reduced padding */
        border-radius: 4px;
        border: 1px solid #cbd5e1;
        user-select: none;
        flex: 1;
        text-align: center;
        height: 36px; /* Fixed height for consistency */
        display: flex;
        align-items: center;
        justify-content: center;
      }

      .refresh-captcha {
        background: #f1f5f9;
        color: #475569;
        border: 1px solid #cbd5e1;
        padding: 8px 12px;
        border-radius: 4px;
        cursor: pointer;
        font-size: 14px;
        transition: all 0.2s;
      }

      .refresh-captcha:hover {
        background: #e2e8f0;
      }

      .captcha-input {
        margin-top: 12px;
      }

      .login-button {
        width: 100%;
        padding: 12px;
        background-color: #2563eb;
        color: white;
        border: none;
        border-radius: 4px;
        font-size: 16px;
        font-weight: 500;
        cursor: pointer;
        transition: background-color 0.2s;
        margin-top: 10px;
      }

      .login-button:hover {
        background-color: #1d4ed8;
      }

      .system-notice {
        margin-top: 20px;
        padding: 12px;
        background: #fefce8;
        border: 1px solid #fef08a;
        border-radius: 4px;
        font-size: 13px;
        color: #854d0e;
      }

      .footer {
        margin-top: 25px;
        text-align: center;
        font-size: 12px;
        color: #6b7280;
        padding-top: 15px;
        border-top: 1px solid #e5e7eb;
      }

      @media (max-width: 480px) {
        .login-container {
          padding: 20px;
        }
      }
    </style>
  </head>
  <body>
    <div class="login-container">
      <div class="login-header">
        <h1>Army Digital Information System</h1>
        <p>Enter your credentials to access the portal</p>
      </div>

      <form id="loginForm">
        <div class="input-group">
          <label for="username">Username</label>
          <input
            type="text"
            id="username"
            placeholder="Enter your username"
            required
          />
        </div>

        <div class="input-group">
          <label for="password">Password</label>
          <div class="password-input">
            <input
              type="password"
              id="password"
              placeholder="Enter your password"
              required
            />
            <button type="button" class="toggle-password" id="togglePassword">
              <i class="fas fa-eye"></i>
            </button>
          </div>
        </div>

        <div class="input-group">
          <label for="role">Access Role</label>
          <select id="role" required>
            <option value="">-- Select access role --</option>
            <option value="admin">Administrator</option>
            <option value="user">Standard User</option>
          </select>
        </div>

        <div class="captcha-container" id="captchaContainer">
          <label for="captchaInput">Enter Captcha</label>
          <div class="captcha-row">
            <div class="captcha-display" id="captchaDisplay"></div>
            <button
              type="button"
              class="refresh-captcha"
              id="refreshCaptcha"
              title="Generate new code"
            >
              <i class="fas fa-sync-alt"></i>
            </button>
          </div>
          <div class="input-group captcha-input">
            <input
              type="text"
              id="captchaInput"
              placeholder="Enter the code above"
              required
            />
          </div>
        </div>

        <button type="submit" class="login-button">Sign In</button>
      </form>

      <div class="system-notice">
        <i class="fas fa-shield-alt"></i> This is a secure government system.
        Unauthorized access is prohibited.
      </div>

      <div class="footer">
        <p>Official Government Portal • v3.2.1</p>
        <p>For assistance, contact IT Support at (555) 123-HELP</p>
      </div>
    </div>

    <script>
      document.addEventListener("DOMContentLoaded", function () {
        // DOM Elements
        const loginForm = document.getElementById("loginForm");
        const roleSelect = document.getElementById("role");
        const captchaContainer = document.getElementById("captchaContainer");
        const captchaDisplay = document.getElementById("captchaDisplay");
        const captchaInput = document.getElementById("captchaInput");
        const refreshCaptchaBtn = document.getElementById("refreshCaptcha");
        const togglePasswordBtn = document.getElementById("togglePassword");
        const passwordInput = document.getElementById("password");

        // Generate CAPTCHA
        function generateCaptcha() {
          const characters = "ABCDEFGHJKLMNPQRSTUVWXYZ23456789";
          let captcha = "";
          for (let i = 0; i < 5; i++) {
            captcha += characters.charAt(
              Math.floor(Math.random() * characters.length)
            );
          }
          captchaDisplay.textContent = captcha;
          captchaInput.value = "";
          return captcha;
        }

        let currentCaptcha = generateCaptcha();

        // Toggle CAPTCHA visibility based on role
        roleSelect.addEventListener("change", function () {
          if (roleSelect.value === "user") {
            captchaContainer.style.display = "block";
            generateCaptcha();
          } else {
            captchaContainer.style.display = "none";
          }
        });

        // Refresh CAPTCHA
        refreshCaptchaBtn.addEventListener("click", function () {
          currentCaptcha = generateCaptcha();
        });

        // Toggle password visibility
        togglePasswordBtn.addEventListener("click", function () {
          if (passwordInput.type === "password") {
            passwordInput.type = "text";
            togglePasswordBtn.innerHTML = '<i class="fas fa-eye-slash"></i>';
          } else {
            passwordInput.type = "password";
            togglePasswordBtn.innerHTML = '<i class="fas fa-eye"></i>';
          }
        });

        // Form submission
        loginForm.addEventListener("submit", function (e) {
          e.preventDefault();

          const username = document.getElementById("username").value;
          const password = document.getElementById("password").value;
          const role = document.getElementById("role").value;

          // Validate form
          if (!username || !password || !role) {
            alert("Please complete all required fields.");
            return;
          }

          // Validate CAPTCHA for users
          if (role === "user") {
            if (captchaInput.value !== currentCaptcha) {
              alert(
                "Security verification failed. Please check the code and try again."
              );
              currentCaptcha = generateCaptcha();
              return;
            }
          }

          // If everything is valid
          alert("Authentication successful. Welcome, " + username + "!");

          // In a real application, you would send the data to a server here
          console.log("Login attempted with:", { username, password, role });
        });
      });
    </script>
  </body>
</html>
