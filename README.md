
<!-- Supabase SDK: add inside <head> -->
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

<h2>SafeView - Private PPT Upload</h2>

<input id="email" type="email" placeholder="Email address">
<input id="password" type="password" placeholder="Password">

<button onclick="signup()">Sign up</button>
<button onclick="login()">Log in</button>

<hr>

<input id="pptFile" type="file"
       accept=".ppt,.pptx,application/vnd.ms-powerpoint,application/vnd.openxmlformats-officedocument.presentationml.presentation">

<button onclick="uploadPPT()">Upload PPT</button>
<button onclick="logout()">Log out</button>

<p id="status"></p>

<script>
  const SUPABASE_URL = "YOUR_PROJECT_URL";
  const SUPABASE_KEY = "YOUR_PUBLISHABLE_KEY";

  const sb = window.supabase.createClient(
    SUPABASE_URL,
    SUPABASE_KEY
  );

  const statusBox = document.getElementById("status");

  async function signup() {
    const { error } = await sb.auth.signUp({
      email: document.getElementById("email").value.trim(),
      password: document.getElementById("password").value
    });

    statusBox.textContent = error
      ? error.message
      : "Signup sent. Verify your email if required.";
  }

  async function login() {
    const { error } = await sb.auth.signInWithPassword({
      email: document.getElementById("email").value.trim(),
      password: document.getElementById("password").value
    });

    statusBox.textContent = error
      ? error.message
      : "Login successful.";
  }

  async function uploadPPT() {
    const { data: sessionData } = await sb.auth.getSession();
    const user = sessionData.session?.user;

    if (!user) {
      statusBox.textContent = "Please log in first.";
      return;
    }

    const file = document.getElementById("pptFile").files[0];

    if (!file) {
      statusBox.textContent = "Select a PPT file first.";
      return;
    }

    const allowedTypes = [
      "application/vnd.ms-powerpoint",
      "application/vnd.openxmlformats-officedocument.presentationml.presentation"
    ];

    const ext = file.name.split(".").pop().toLowerCase();

    if (!["ppt", "pptx"].includes(ext) ||
        !allowedTypes.includes(file.type)) {
      statusBox.textContent = "Select a valid PPT or PPTX file.";
      return;
    }

    // Demo limit; set a matching Storage bucket limit too.
    if (file.size > 10 * 1024 * 1024) {
      statusBox.textContent = "Maximum upload size is 10 MB.";
      return;
    }

    const safeName = crypto.randomUUID() + "." + ext;
    const path = user.id + "/" + safeName;

    const { error } = await sb.storage
      .from("private-ppts")
      .upload(path, file, {
        upsert: false,
        contentType: file.type
      });

    if (error) {
      statusBox.textContent = "Upload failed: " + error.message;
      return;
    }

    const { error: dbError } = await sb
      .from("ppt_files")
      .insert({
        owner_id: user.id,
        title: file.name,
        storage_path: path
      });

    if (dbError) {
      statusBox.textContent =
        "File uploaded, but database save failed. Contact admin.";
      return;
    }

    statusBox.textContent = "PPT uploaded privately!";
  }

  async function logout() {
    const { error } = await sb.auth.signOut();
    statusBox.textContent = error
      ? error.message
      : "Logged out.";
  }
</script>
