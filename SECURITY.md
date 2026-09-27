<?php
// (تكملة لملف login-process.php بعد نجاح التحقق من كلمة المرور)
if ($username === $stored_username && password_verify($password, $stored_hashed_password)) {
    
    // تجديد معرف الجلسة أماناً
    session_regenerate_id(true);

    // 1. توليد رمز تحقق ثنائي عشوائي مكون من 6 أرقام
    $otp_code = random_int(100000, 999999);

    // 2. تخزين الرمز ووقت انتهائه في الجلسة (صالح لمدة 5 دقائق)
    $_SESSION['temp_user'] = $username;
    $_SESSION['otp_code'] = $otp_code;
    $_SESSION['otp_expires'] = time() + 300; // 5 دقائق

    // 3. إرسال الرمز عبر البريد الإلكتروني (مثال باستخدام دالة mail في PHP)
    $to = "user@example.com"; // بريد المستخدم المسجل في قاعدة البيانات
    $subject = "رمز التحقق الثنائي (2FA)";
    $message = "رمز التحقق الخاص بك هو: $otp_code \nالرمز صالح لمدة 5 دقائق فقط.";
    $headers = "From: no-reply@yoursite.com\r\n";
    
    // (اختياري حقيقي): mail($to, $subject, $message, $headers);

    // 4. التوجيه إلى صفحة إدخال رمز التحقق الثنائي
    header("Location: verify-2fa.php");
    exit();
}
