[index.html](https://github.com/user-attachments/files/27232393/index.html)
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Court Marriage Certificate System</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, sans-serif; }
  body { background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%); min-height: 100vh; color: #333; }
  .hidden { display: none !important; }

  /* ========= NAVBAR ========= */
  .navbar {
    background: #fff; padding: 12px 30px; display: flex;
    justify-content: space-between; align-items: center;
    box-shadow: 0 2px 10px rgba(0,0,0,0.15);
    position: sticky; top: 0; z-index: 50;
  }
  .navbar .logo { font-size: 22px; font-weight: 700; color: #1e3c72; display: flex; align-items: center; gap: 10px; }
  .navbar .logo span.icon { font-size: 28px; }
  .navbar .nav-actions { display: flex; gap: 12px; align-items: center; }
  .nav-user { font-size: 14px; color: #555; }
  .btn-logout {
    background: #e74c3c; color: #fff; border: none;
    padding: 8px 16px; border-radius: 6px; cursor: pointer;
    font-size: 14px; transition: 0.2s;
  }
  .btn-logout:hover { background: #c0392b; }

  /* ========= LOGIN ========= */
  .login-wrap {
    min-height: 100vh; display: flex; align-items: center; justify-content: center; padding: 20px;
  }
  .login-card {
    background: #fff; border-radius: 16px; padding: 40px;
    width: 100%; max-width: 420px;
    box-shadow: 0 20px 60px rgba(0,0,0,0.3);
  }
  .login-card .header { text-align: center; margin-bottom: 30px; }
  .login-card .header .emblem { font-size: 60px; margin-bottom: 10px; }
  .login-card h1 { color: #1e3c72; font-size: 24px; margin-bottom: 6px; }
  .login-card .subtitle { color: #777; font-size: 14px; }
  .form-group { margin-bottom: 18px; }
  .form-group label { display: block; font-size: 13px; color: #444; margin-bottom: 6px; font-weight: 600; }
  .form-group input, .form-group select, .form-group textarea {
    width: 100%; padding: 11px 14px; border: 1.5px solid #ddd;
    border-radius: 8px; font-size: 14px; transition: 0.2s;
  }
  .form-group input:focus, .form-group select:focus, .form-group textarea:focus {
    border-color: #2a5298; outline: none; box-shadow: 0 0 0 3px rgba(42,82,152,0.12);
  }
  .btn-primary {
    background: linear-gradient(135deg, #1e3c72, #2a5298); color: #fff;
    border: none; padding: 12px 24px; border-radius: 8px;
    font-size: 15px; font-weight: 600; cursor: pointer; width: 100%;
    transition: 0.2s;
  }
  .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 20px rgba(42,82,152,0.4); }
  .login-hint {
    background: #fff8e1; padding: 10px 14px; border-radius: 8px;
    margin-top: 16px; font-size: 12px; color: #6d4c00; border-left: 3px solid #ffb300;
  }
  .alert-error { background: #fdecea; color: #b71c1c; padding: 10px 14px; border-radius: 8px; font-size: 13px; margin-bottom: 14px; border-left: 3px solid #b71c1c; }
  .alert-success { background: #e8f5e9; color: #1b5e20; padding: 10px 14px; border-radius: 8px; font-size: 13px; margin-bottom: 14px; border-left: 3px solid #1b5e20; }

  /* ========= DASHBOARD ========= */
  .container { max-width: 1100px; margin: 0 auto; padding: 30px 20px; }
  .page-title { color: #fff; font-size: 28px; margin-bottom: 6px; }
  .page-subtitle { color: rgba(255,255,255,0.85); margin-bottom: 24px; font-size: 14px; }
  .dashboard-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 20px; margin-bottom: 30px; }
  .stat-card {
    background: #fff; border-radius: 14px; padding: 22px;
    box-shadow: 0 8px 25px rgba(0,0,0,0.1);
    display: flex; align-items: center; gap: 16px;
  }
  .stat-card .icon { font-size: 36px; }
  .stat-card .label { color: #777; font-size: 13px; }
  .stat-card .value { color: #1e3c72; font-size: 24px; font-weight: 700; }

  .actions-row { display: flex; gap: 12px; flex-wrap: wrap; margin-bottom: 24px; }
  .action-btn {
    background: #fff; color: #1e3c72; border: none;
    padding: 14px 22px; border-radius: 10px; cursor: pointer;
    font-size: 15px; font-weight: 600;
    box-shadow: 0 6px 18px rgba(0,0,0,0.12); transition: 0.2s;
    display: inline-flex; align-items: center; gap: 8px;
  }
  .action-btn:hover { transform: translateY(-2px); box-shadow: 0 10px 24px rgba(0,0,0,0.18); }
  .action-btn.primary { background: linear-gradient(135deg, #ff9800, #f57c00); color: #fff; }

  .records-table-wrap {
    background: #fff; border-radius: 14px; padding: 20px;
    box-shadow: 0 8px 25px rgba(0,0,0,0.1);
  }
  .records-table-wrap h2 { color: #1e3c72; font-size: 18px; margin-bottom: 14px; }
  table { width: 100%; border-collapse: collapse; font-size: 14px; }
  th, td { padding: 12px; text-align: left; border-bottom: 1px solid #eee; }
  th { background: #f5f7fb; color: #555; font-weight: 600; font-size: 13px; }
  .table-actions button { padding: 6px 10px; font-size: 12px; border: none; border-radius: 5px; cursor: pointer; margin-right: 4px; }
  .btn-view { background: #2a5298; color: #fff; }
  .btn-edit { background: #f57c00; color: #fff; }
  .btn-delete { background: #e74c3c; color: #fff; }
  .empty-row { text-align: center; padding: 30px; color: #888; }

  /* ========= FORM ========= */
  .form-card {
    background: #fff; border-radius: 14px; padding: 28px;
    box-shadow: 0 8px 25px rgba(0,0,0,0.15); margin-bottom: 24px;
  }
  .form-section-title {
    color: #1e3c72; font-size: 18px; font-weight: 700; margin-bottom: 16px;
    padding-bottom: 8px; border-bottom: 2px solid #1e3c72;
    display: flex; align-items: center; gap: 8px;
  }
  .form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 14px; }
  .photo-upload {
    border: 2px dashed #2a5298; border-radius: 10px; padding: 14px;
    text-align: center; background: #f5f8ff; cursor: pointer; transition: 0.2s;
  }
  .photo-upload:hover { background: #ebf2ff; }
  .photo-upload img { max-width: 120px; max-height: 140px; border-radius: 6px; margin-top: 8px; }
  .photo-upload input { display: none; }
  .photo-upload .plabel { color: #2a5298; font-weight: 600; font-size: 13px; }
  .photo-upload .pnote { color: #888; font-size: 11px; }

  .aadhaar-row { display: flex; gap: 8px; align-items: end; }
  .aadhaar-row > .form-group { flex: 1; margin-bottom: 0; }
  .btn-fetch {
    padding: 11px 16px; background: #00897b; color: #fff;
    border: none; border-radius: 8px; cursor: pointer; font-size: 13px; font-weight: 600;
    white-space: nowrap;
  }
  .btn-fetch:hover { background: #00695c; }

  .form-actions { display: flex; gap: 12px; justify-content: flex-end; margin-top: 20px; }
  .btn-secondary {
    background: #eceff1; color: #455a64; border: none;
    padding: 12px 22px; border-radius: 8px; cursor: pointer; font-weight: 600;
  }

  /* ========= CERTIFICATE — ARYA SAMAJ ========= */
  .cert-page-actions { display: flex; gap: 10px; flex-wrap: wrap; margin-bottom: 16px; }

  .certificate {
    background: #fff;
    width: 794px;          /* A4 width @ 96 DPI */
    min-height: 1123px;    /* A4 height @ 96 DPI */
    margin: 0 auto;
    box-shadow: 0 10px 40px rgba(0,0,0,0.3);
    color: #1a1a1a; font-family: 'Georgia', serif;
    position: relative; overflow: hidden;
    box-sizing: border-box;
  }
  @media (max-width: 820px) {
    .certificate {
      width: 100%; min-height: auto;
      transform-origin: top center;
    }
  }

  /* Swastika border strips - Hindu auspicious symbol */
  .swastika-h {
    height: 30px;
    background-color: #FFD600;
    background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><g fill='%23C62828'><rect x='40' y='0' width='20' height='100'/><rect x='0' y='40' width='100' height='20'/><rect x='0' y='0' width='40' height='20'/><rect x='80' y='0' width='20' height='40'/><rect x='60' y='80' width='40' height='20'/><rect x='0' y='60' width='20' height='40'/></g></svg>");
    background-repeat: repeat-x;
    background-size: 30px 30px;
    background-position: center;
  }
  .swastika-v {
    width: 30px;
    background-color: #FFD600;
    background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><g fill='%23C62828'><rect x='40' y='0' width='20' height='100'/><rect x='0' y='40' width='100' height='20'/><rect x='0' y='0' width='40' height='20'/><rect x='80' y='0' width='20' height='40'/><rect x='60' y='80' width='40' height='20'/><rect x='0' y='60' width='20' height='40'/></g></svg>");
    background-repeat: repeat-y;
    background-size: 30px 30px;
    background-position: center;
  }

  .cert-row { display: flex; }
  .cert-content {
    flex: 1; padding: 20px 26px;
    position: relative; background: #fff;
  }

  /* ओ३म् watermark in background */
  .cert-content::before {
    content: 'ॐ';
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    font-size: 380px;
    color: rgba(198, 40, 40, 0.06);
    pointer-events: none;
    font-family: 'Mangal', 'Nirmala UI', serif;
    z-index: 0;
    line-height: 1;
  }
  .cert-content > * { position: relative; z-index: 1; }

  .cert-om-top {
    text-align: center;
    font-size: 22px;
    color: #C62828;
    font-weight: bold;
    margin: 0 0 2px;
    letter-spacing: 2px;
  }
  .cert-org-title {
    text-align: center;
    font-size: 30px;
    color: #C62828;
    font-weight: 900;
    letter-spacing: 1.5px;
    font-style: italic;
    margin: 4px 0 6px;
    text-shadow: 2px 2px 0 rgba(255,214,0,0.5);
    font-family: 'Georgia', serif;
  }
  .cert-address {
    text-align: center;
    font-size: 11.5px;
    color: #222;
    line-height: 1.55;
    margin-bottom: 8px;
  }
  .cert-address div { margin: 1px 0; }

  .cert-top-grid {
    display: grid;
    grid-template-columns: 110px 1fr 110px;
    gap: 14px;
    align-items: center;
    margin: 10px 0 6px;
  }
  .cert-side-photo-wrap { text-align: center; }
  .cert-side-photo {
    width: 100px; height: 120px;
    border: 2px solid #1a237e;
    background: #f5f5f5;
    display: flex; align-items: center; justify-content: center;
    overflow: hidden;
    font-size: 11px;
    color: #999;
    margin: 0 auto;
  }
  .cert-side-photo img { width: 100%; height: 100%; object-fit: cover; }
  .cert-side-label {
    margin-top: 4px;
    font-size: 11px;
    font-weight: 700;
    color: #1a237e;
    font-family: 'Mangal', 'Nirmala UI', sans-serif;
  }

  .cert-banner-wrap { text-align: center; }
  .cert-banner {
    display: inline-block;
    background: linear-gradient(135deg, #d32f2f, #b71c1c);
    color: #fff;
    padding: 8px 32px;
    border-radius: 4px;
    font-size: 19px;
    font-style: italic;
    font-weight: 700;
    box-shadow: 0 4px 8px rgba(0,0,0,0.25);
    letter-spacing: 1px;
    border: 2px solid #fff;
    outline: 1px solid #b71c1c;
  }
  .cert-banner-hindi {
    font-size: 13px;
    color: #c62828;
    margin-top: 6px;
    margin-bottom: 4px;
    font-weight: 600;
    font-family: 'Mangal', 'Nirmala UI', sans-serif;
  }
  .cert-stamp-inline {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 4px;
  }
  .cert-stamp-inline svg { display: block; }

  /* Bottom-corner big stamp */
  .cert-bottom-stamp-wrap {
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    margin-left: auto;
    transform: rotate(-6deg);
  }
  .cert-bottom-stamp-wrap svg { display: block; }

  /* Group wedding photo block */
  .cert-group-photo-wrap {
    text-align: center;
    margin: 16px 0 10px;
    padding: 10px;
    border: 1px dashed #1a237e;
    background: rgba(26,35,126,0.03);
    border-radius: 6px;
  }
  .cert-group-photo {
    width: 100%;
    max-width: 460px;
    height: 200px;
    border: 2px solid #1a237e;
    background: #f0f0f0;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }
  .cert-group-photo img { width: 100%; height: 100%; object-fit: cover; }
  .cert-group-label {
    font-size: 11px;
    color: #1a237e;
    font-weight: 700;
    margin-top: 6px;
    letter-spacing: 0.5px;
  }

  /* Digital signature style block (Arya Samaj branded, not government) */
  .cert-digisign {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-top: 12px;
    padding: 8px 12px;
    border: 1.5px solid #1a237e;
    border-left: 5px solid #1a237e;
    background: #f0f4ff;
    border-radius: 4px;
  }
  .cert-digisign-icon {
    width: 38px; height: 38px;
    background: #1a237e;
    color: #fff;
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 22px;
    font-weight: 700;
    flex-shrink: 0;
  }
  .cert-digisign-text {
    font-size: 12px;
    color: #1a237e;
    line-height: 1.5;
    flex: 1;
  }

  /* QR Code on certificate */
  .cert-qr-block {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    flex-shrink: 0;
  }
  .cert-qr-block .qr-wrap {
    width: 95px; height: 95px;
    padding: 4px;
    background: #fff;
    border: 1.5px solid #1a237e;
    border-radius: 4px;
  }
  .cert-qr-block .qr-wrap img,
  .cert-qr-block .qr-wrap canvas {
    width: 100% !important; height: 100% !important;
    display: block;
  }
  .cert-qr-block .qr-label {
    font-size: 9px;
    color: #1a237e;
    font-weight: 700;
    text-align: center;
    line-height: 1.2;
  }

  /* ========= VERIFICATION PAGE ========= */
  .verify-wrap {
    min-height: 100vh;
    display: flex; align-items: center; justify-content: center;
    padding: 20px;
    background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
  }
  .verify-card {
    background: #fff;
    max-width: 700px;
    width: 100%;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 20px 60px rgba(0,0,0,0.3);
  }
  .verify-header {
    background: linear-gradient(135deg, #1b5e20, #2e7d32);
    color: #fff;
    padding: 30px 24px;
    text-align: center;
  }
  .verify-icon {
    width: 72px; height: 72px;
    background: #fff;
    color: #2e7d32;
    border-radius: 50%;
    display: inline-flex;
    align-items: center; justify-content: center;
    font-size: 44px;
    font-weight: 900;
    margin-bottom: 10px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.2);
  }
  .verify-header h1 { font-size: 24px; margin-bottom: 6px; }
  .verify-header p { font-size: 14px; opacity: 0.95; }
  .verify-body { padding: 24px; }
  .verify-cert-no {
    text-align: center;
    background: #fff5cc;
    border: 1px dashed #c62828;
    padding: 12px;
    border-radius: 8px;
    margin-bottom: 18px;
  }
  .verify-cert-no .label { font-size: 11px; color: #777; }
  .verify-cert-no .value { font-size: 22px; font-weight: 800; color: #c62828; letter-spacing: 1px; }
  .verify-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 18px;
  }
  .verify-card-inner {
    border: 1px solid #ddd; border-radius: 8px;
    padding: 14px; background: #fafafa;
  }
  .verify-card-inner h3 {
    font-size: 14px; color: #1a237e;
    margin-bottom: 10px; padding-bottom: 6px;
    border-bottom: 1.5px solid #1a237e;
  }
  .verify-card-inner p { font-size: 13px; margin: 4px 0; line-height: 1.5; }
  .verify-card-inner p b { color: #555; }
  .verify-meta-row {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 12px; margin-bottom: 18px; font-size: 13px;
  }
  .verify-meta-row > div {
    background: #f0f4ff;
    padding: 10px;
    border-radius: 6px;
    border-left: 3px solid #1a237e;
  }
  .verify-witnesses {
    background: #fafafa;
    padding: 14px;
    border-radius: 8px;
    border: 1px solid #ddd;
    margin-bottom: 18px;
    font-size: 13px;
  }
  .verify-witnesses h3 { color: #1a237e; font-size: 14px; margin-bottom: 8px; }
  .verify-footer {
    text-align: center;
    padding: 14px;
    background: #f0f4ff;
    color: #1a237e;
    font-size: 11px;
    border-top: 2px solid #1a237e;
  }
  .verify-footer button {
    margin-top: 8px;
    padding: 8px 16px;
    background: #1a237e;
    color: #fff;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-size: 13px;
    font-weight: 600;
  }
  .verify-error {
    background: #ffebee;
    color: #b71c1c;
    padding: 30px;
    text-align: center;
    font-size: 16px;
    border-radius: 8px;
  }
  .verify-error h2 { margin-bottom: 10px; font-size: 22px; }
  @media (max-width: 600px) {
    .verify-grid, .verify-meta-row { grid-template-columns: 1fr; }
  }

  .cert-info-row {
    display: flex; justify-content: space-between;
    font-size: 12.5px; margin: 8px 0 4px;
    flex-wrap: wrap; gap: 8px;
  }
  .cert-info-row > div { padding: 2px 6px; }

  .cert-ref {
    font-size: 13px;
    margin: 6px 0 4px;
  }
  .cert-ref-line {
    display: inline-block;
    border-bottom: 1px dotted #555;
    min-width: 220px;
    padding: 0 6px;
    font-weight: 700;
    color: #b71c1c;
  }

  .cert-statement {
    text-align: center;
    font-style: italic;
    font-size: 14px;
    margin: 10px 0 8px;
    padding: 6px 10px;
    color: #222;
    line-height: 1.5;
  }

  .cert-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 11px;
    margin: 6px 0 10px;
  }
  .cert-table th, .cert-table td {
    border: 1px solid #555;
    padding: 6px 5px;
    text-align: left;
    vertical-align: top;
  }
  .cert-table th {
    background: #fff5cc;
    text-align: center;
    font-size: 10.5px;
    font-weight: 700;
    color: #444;
    letter-spacing: 0.5px;
  }
  .cert-table .row-label {
    background: #fff5cc;
    font-weight: 700;
    text-align: center;
    font-size: 10.5px;
    width: 80px;
    vertical-align: middle;
  }
  .cert-table .name-cell { display: flex; align-items: center; gap: 6px; }
  .cert-table .mini-photo {
    width: 28px; height: 32px;
    border: 1px solid #555;
    object-fit: cover;
    flex-shrink: 0;
  }

  .cert-witnesses {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
    margin: 10px 0;
    font-size: 12px;
  }
  .witness-block {
    border-left: 3px solid #c62828;
    padding-left: 10px;
    background: rgba(255,245,204,0.25);
    padding-top: 4px; padding-bottom: 4px;
  }
  .witness-block h4 {
    color: #c62828; margin-bottom: 4px; font-size: 13px;
    font-style: italic;
  }
  .witness-block p { margin: 3px 0; }
  .witness-block .line {
    display: inline-block;
    border-bottom: 1px dotted #555;
    min-width: 130px;
    padding: 0 4px;
    font-weight: 600;
  }

  .cert-warning {
    font-size: 10.5px;
    color: #444;
    font-style: italic;
    text-align: justify;
    margin: 12px 0 8px;
    padding: 8px 10px;
    border: 1px dashed #c62828;
    border-radius: 4px;
    background: #fffaf0;
    line-height: 1.5;
  }
  .cert-warning b { color: #c62828; font-style: normal; }

  .cert-bottom-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    margin-top: 16px;
    align-items: end;
    gap: 16px;
  }
  .cert-pandit-sign {
    text-align: left;
    font-size: 11px;
    color: #333;
    font-weight: 600;
  }
  .cert-pandit-sign .sign-line-top {
    border-top: 1px solid #555;
    width: 200px;
    margin-bottom: 4px;
    margin-top: 30px;
  }

  /* ========= UTILITY ========= */
  .small-note { font-size: 11px; color: #888; margin-top: 4px; }
  .age-display { font-size: 12px; color: #555; margin-top: 4px; font-weight: 600; }
  .age-display.error { color: #c62828; }
  .age-display.ok { color: #2e7d32; }
  @media (max-width: 700px) {
    .cert-content { padding: 14px; }
    .cert-witnesses { grid-template-columns: 1fr; }
    .cert-bottom-row { grid-template-columns: 1fr; }
    .cert-bottom-stamp-wrap { margin: 0 auto; transform: none; }
    .cert-group-photo { height: 160px; }
    .cert-org-title { font-size: 22px; }
    .cert-content::before { font-size: 220px; }
    .cert-table { font-size: 10px; }
    .cert-top-grid { grid-template-columns: 1fr; }
  }

  @media print {
    body * { visibility: hidden; }
    .certificate, .certificate * { visibility: visible; }
    .certificate { position: absolute; left: 0; top: 0; box-shadow: none; border: 8px double #1e3c72; }
    .navbar, .cert-page-actions { display: none !important; }
  }
</style>
</head>
<body>

<!-- ========= LOGIN PAGE ========= -->
<div id="loginPage" class="login-wrap">
  <div class="login-card">
    <div class="header">
      <div class="emblem">⚖️</div>
      <h1>Court Marriage Certificate</h1>
      <div class="subtitle">Online Certificate Generation System</div>
    </div>
    <div id="loginAlert"></div>
    <form id="loginForm" onsubmit="return doLogin(event)">
      <div class="form-group">
        <label>Username</label>
        <input type="text" id="loginUser" required placeholder="Enter username" autocomplete="username"/>
      </div>
      <div class="form-group">
        <label>Password</label>
        <input type="password" id="loginPass" required placeholder="Enter password" autocomplete="current-password"/>
      </div>
      <button type="submit" class="btn-primary">🔐 Login</button>
    </form>
    <div class="login-hint">
      <strong>Default login:</strong> Username: <b>admin</b> | Password: <b>admin123</b><br/>
      Login ke baad aap password change kar sakte hain (Settings se).
    </div>
  </div>
</div>

<!-- ========= APP SHELL ========= -->
<div id="appShell" class="hidden">
  <div class="navbar">
    <div class="logo"><span class="icon">⚖️</span> Court Marriage Certificate</div>
    <div class="nav-actions">
      <span class="nav-user" id="navUser">Welcome</span>
      <button class="btn-logout" onclick="showSettings()">⚙️ Settings</button>
      <button class="btn-logout" onclick="doLogout()">Logout</button>
    </div>
  </div>

  <!-- ========= DASHBOARD ========= -->
  <div id="dashboardPage" class="container">
    <h1 class="page-title">Dashboard</h1>
    <p class="page-subtitle">Marriage Certificate Records & New Application</p>

    <div class="dashboard-grid">
      <div class="stat-card"><div class="icon">📜</div><div><div class="label">Total Certificates</div><div class="value" id="statTotal">0</div></div></div>
      <div class="stat-card"><div class="icon">👥</div><div><div class="label">Saved Profiles (Aadhaar)</div><div class="value" id="statProfiles">0</div></div></div>
      <div class="stat-card"><div class="icon">📅</div><div><div class="label">This Month</div><div class="value" id="statMonth">0</div></div></div>
    </div>

    <div class="actions-row">
      <button class="action-btn primary" onclick="showForm()">➕ New Certificate Application</button>
      <button class="action-btn" onclick="loadDashboard()">🔄 Refresh</button>
    </div>

    <div class="records-table-wrap">
      <h2>📋 Saved Certificates</h2>
      <table>
        <thead>
          <tr>
            <th>Cert No.</th>
            <th>Groom</th>
            <th>Bride</th>
            <th>Marriage Date</th>
            <th>Created</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody id="recordsBody">
          <tr><td colspan="6" class="empty-row">No certificates yet. Click "New Certificate Application" to start.</td></tr>
        </tbody>
      </table>
    </div>
  </div>

  <!-- ========= APPLICATION FORM ========= -->
  <div id="formPage" class="container hidden">
    <h1 class="page-title">New Marriage Certificate Application</h1>
    <p class="page-subtitle">Fill all the details carefully. Aadhaar should be 12 digits. Photos optional but recommended.</p>

    <form id="appForm" onsubmit="return generateCertificate(event)">

      <!-- GROOM -->
      <div class="form-card">
        <div class="form-section-title">🤵 Groom (वर) Details</div>
        <div class="aadhaar-row">
          <div class="form-group">
            <label>Aadhaar Number *</label>
            <input type="text" id="g_aadhaar" maxlength="12" pattern="\d{12}" required placeholder="12 digit Aadhaar"/>
            <div class="small-note">If profile already saved, click Fetch to auto-fill.</div>
          </div>
          <button type="button" class="btn-fetch" onclick="fetchProfile('g')">📥 Fetch</button>
        </div>
        <div class="form-grid">
          <div class="form-group"><label>Full Name *</label><input type="text" id="g_name" required/></div>
          <div class="form-group"><label>Father's Name *</label><input type="text" id="g_father" required/></div>
          <div class="form-group"><label>Mother's Name *</label><input type="text" id="g_mother" required/></div>
          <div class="form-group"><label>Date of Birth *</label><input type="date" id="g_dob" required onchange="calcAge('g')"/><div id="g_age_disp" class="age-display"></div></div>
          <div class="form-group"><label>Religion *</label>
            <select id="g_religion" required>
              <option value="">Select</option><option>Hindu</option><option>Muslim</option><option>Sikh</option><option>Christian</option><option>Jain</option><option>Buddhist</option><option>Other</option>
            </select>
          </div>
          <div class="form-group"><label>Occupation</label><input type="text" id="g_occupation"/></div>
          <div class="form-group"><label>Mobile Number *</label><input type="tel" id="g_mobile" maxlength="10" pattern="\d{10}" required/></div>
          <div class="form-group" style="grid-column: 1/-1;"><label>Address *</label><textarea id="g_address" rows="2" required></textarea></div>
          <div>
            <label style="font-size: 13px; color: #444; font-weight: 600;">Photo (Passport size)</label>
            <label class="photo-upload" for="g_photo">
              <div class="plabel">📷 Upload Groom Photo</div>
              <div class="pnote">Click to select image</div>
              <img id="g_photo_preview" style="display:none"/>
              <input type="file" id="g_photo" accept="image/*" onchange="previewPhoto('g_photo','g_photo_preview')"/>
            </label>
          </div>
        </div>
      </div>

      <!-- BRIDE -->
      <div class="form-card">
        <div class="form-section-title">👰 Bride (वधू) Details</div>
        <div class="aadhaar-row">
          <div class="form-group">
            <label>Aadhaar Number *</label>
            <input type="text" id="b_aadhaar" maxlength="12" pattern="\d{12}" required placeholder="12 digit Aadhaar"/>
          </div>
          <button type="button" class="btn-fetch" onclick="fetchProfile('b')">📥 Fetch</button>
        </div>
        <div class="form-grid">
          <div class="form-group"><label>Full Name *</label><input type="text" id="b_name" required/></div>
          <div class="form-group"><label>Father's Name *</label><input type="text" id="b_father" required/></div>
          <div class="form-group"><label>Mother's Name *</label><input type="text" id="b_mother" required/></div>
          <div class="form-group"><label>Date of Birth *</label><input type="date" id="b_dob" required onchange="calcAge('b')"/><div id="b_age_disp" class="age-display"></div></div>
          <div class="form-group"><label>Religion *</label>
            <select id="b_religion" required>
              <option value="">Select</option><option>Hindu</option><option>Muslim</option><option>Sikh</option><option>Christian</option><option>Jain</option><option>Buddhist</option><option>Other</option>
            </select>
          </div>
          <div class="form-group"><label>Occupation</label><input type="text" id="b_occupation"/></div>
          <div class="form-group"><label>Mobile Number *</label><input type="tel" id="b_mobile" maxlength="10" pattern="\d{10}" required/></div>
          <div class="form-group" style="grid-column: 1/-1;"><label>Address *</label><textarea id="b_address" rows="2" required></textarea></div>
          <div>
            <label style="font-size: 13px; color: #444; font-weight: 600;">Photo (Passport size)</label>
            <label class="photo-upload" for="b_photo">
              <div class="plabel">📷 Upload Bride Photo</div>
              <div class="pnote">Click to select image</div>
              <img id="b_photo_preview" style="display:none"/>
              <input type="file" id="b_photo" accept="image/*" onchange="previewPhoto('b_photo','b_photo_preview')"/>
            </label>
          </div>
        </div>
      </div>

      <!-- MARRIAGE DETAILS -->
      <div class="form-card">
        <div class="form-section-title">💍 Marriage Details</div>
        <div class="form-grid">
          <div class="form-group"><label>Date of Marriage *</label><input type="date" id="m_date" required/></div>
          <div class="form-group"><label>Place of Marriage *</label><input type="text" id="m_place" required placeholder="District / City"/></div>
          <div class="form-group"><label>Pandit / Priest Name *</label><input type="text" id="m_officer" required value="Pandit Ji"/></div>
          <div class="form-group"><label>State *</label><input type="text" id="m_state" required value="Delhi"/></div>
          <div class="form-group"><label>Marriage Act / Type *</label>
            <select id="m_act" required>
              <option>Hindu Vedic Rites (Arya Samaj)</option>
              <option>Hindu Marriage Act, 1955</option>
              <option>Special Marriage Act, 1954</option>
            </select>
          </div>
          <div>
            <label style="font-size: 13px; color: #444; font-weight: 600;">Group Wedding Photo (with witnesses)</label>
            <label class="photo-upload" for="m_photo">
              <div class="plabel">📷 Upload Group Photo</div>
              <div class="pnote">Bride, groom & witnesses together</div>
              <img id="m_photo_preview" style="display:none"/>
              <input type="file" id="m_photo" accept="image/*" onchange="previewPhoto('m_photo','m_photo_preview')"/>
            </label>
          </div>
        </div>
      </div>

      <!-- WITNESS 1 -->
      <div class="form-card">
        <div class="form-section-title">👤 Witness 1 (गवाह 1) Details</div>
        <div class="aadhaar-row">
          <div class="form-group">
            <label>Aadhaar Number *</label>
            <input type="text" id="w1_aadhaar" maxlength="12" pattern="\d{12}" required placeholder="12 digit Aadhaar"/>
          </div>
          <button type="button" class="btn-fetch" onclick="fetchProfile('w1')">📥 Fetch</button>
        </div>
        <div class="form-grid">
          <div class="form-group"><label>Full Name *</label><input type="text" id="w1_name" required/></div>
          <div class="form-group"><label>Father's Name *</label><input type="text" id="w1_father" required/></div>
          <div class="form-group"><label>Mobile Number</label><input type="tel" id="w1_mobile" maxlength="10" pattern="\d{10}"/></div>
          <div class="form-group"><label>Relationship to Couple</label><input type="text" id="w1_relation" placeholder="e.g., Friend, Uncle"/></div>
          <div class="form-group" style="grid-column: 1/-1;"><label>Address *</label><textarea id="w1_address" rows="2" required></textarea></div>
        </div>
      </div>

      <!-- WITNESS 2 -->
      <div class="form-card">
        <div class="form-section-title">👤 Witness 2 (गवाह 2) Details</div>
        <div class="aadhaar-row">
          <div class="form-group">
            <label>Aadhaar Number *</label>
            <input type="text" id="w2_aadhaar" maxlength="12" pattern="\d{12}" required placeholder="12 digit Aadhaar"/>
          </div>
          <button type="button" class="btn-fetch" onclick="fetchProfile('w2')">📥 Fetch</button>
        </div>
        <div class="form-grid">
          <div class="form-group"><label>Full Name *</label><input type="text" id="w2_name" required/></div>
          <div class="form-group"><label>Father's Name *</label><input type="text" id="w2_father" required/></div>
          <div class="form-group"><label>Mobile Number</label><input type="tel" id="w2_mobile" maxlength="10" pattern="\d{10}"/></div>
          <div class="form-group"><label>Relationship to Couple</label><input type="text" id="w2_relation" placeholder="e.g., Friend, Uncle"/></div>
          <div class="form-group" style="grid-column: 1/-1;"><label>Address *</label><textarea id="w2_address" rows="2" required></textarea></div>
        </div>
      </div>

      <div id="formAlert"></div>
      <div class="form-actions">
        <button type="button" class="btn-secondary" onclick="showDashboard()">Cancel</button>
        <button type="submit" class="btn-primary" style="width:auto; padding: 12px 30px;">📜 Generate Certificate</button>
      </div>
    </form>
  </div>

  <!-- ========= CERTIFICATE PAGE ========= -->
  <div id="certPage" class="container hidden">
    <div class="cert-page-actions">
      <button class="action-btn" onclick="showDashboard()">⬅ Back</button>
      <button class="action-btn primary" onclick="downloadPDF()">⬇️ Download PDF</button>
      <button class="action-btn" onclick="window.print()">🖨️ Print</button>
    </div>

    <div id="certificate" class="certificate">
      <!-- TOP swastika border -->
      <div class="swastika-h"></div>

      <!-- MIDDLE row: left swastika | content | right swastika -->
      <div class="cert-row">
        <div class="swastika-v"></div>

        <div class="cert-content">
          <!-- ओ३म् symbol -->
          <div class="cert-om-top">ॐ ओ३म्</div>

          <!-- Title -->
          <h1 class="cert-org-title">ARYA SAMAJ MARRIAGE TRUST (REGD.)</h1>

          <!-- Address -->
          <div class="cert-address">
            <div><b>Regd. Office:</b> 20, CSC-5, DDA Market, Sector-14, Rohini, New Delhi-110085</div>
            <div><b>Branch:</b> 346, Block-E, PKT-3, Sector-18, Rohini, New Delhi-110089</div>
            <div><b>Mob.:</b> 9911246764 &nbsp;|&nbsp; <b>E-mail:</b> asmtrohini@gmail.com</div>
          </div>

          <!-- Top row: GROOM PHOTO | (Banner + Stamp) | BRIDE PHOTO -->
          <div class="cert-top-grid">
            <div class="cert-side-photo-wrap">
              <div class="cert-side-photo">
                <img id="cert_groom_photo" alt="Groom" style="display:none"/>
                <span id="cert_groom_ph">Groom<br/>Photo</span>
              </div>
              <div class="cert-side-label">वर / GROOM</div>
            </div>
            <div class="cert-banner-wrap">
              <div class="cert-banner">Certificate of Marriage</div>
              <div class="cert-banner-hindi">विवाह संस्कार प्रमाण पत्र</div>
              <div class="cert-stamp-inline">
                <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg" width="100" height="100">
                  <defs>
                    <path id="stampTopArc" d="M 100,100 m -78,0 a 78,78 0 1,1 156,0" fill="none"/>
                  </defs>
                  <circle cx="100" cy="100" r="86" fill="none" stroke="#1a237e" stroke-width="3"/>
                  <circle cx="100" cy="100" r="62" fill="none" stroke="#1a237e" stroke-width="2"/>
                  <text font-size="14" font-weight="bold" fill="#1a237e" font-family="Arial, sans-serif" letter-spacing="0.6">
                    <textPath href="#stampTopArc" startOffset="50%" text-anchor="middle">ARYA SAMAJ MARRIAGE TRUST (REGD.)</textPath>
                  </text>
                  <text x="100" y="90" text-anchor="middle" font-size="14" font-weight="bold" fill="#1a237e" font-family="Arial, sans-serif">Regd. No.</text>
                  <text x="100" y="112" text-anchor="middle" font-size="18" font-weight="bold" fill="#1a237e" font-family="Arial, sans-serif">1212/22</text>
                  <text x="100" y="132" text-anchor="middle" font-size="15" font-weight="bold" fill="#1a237e" font-family="Arial, sans-serif">DELHI</text>
                </svg>
              </div>
            </div>
            <div class="cert-side-photo-wrap">
              <div class="cert-side-photo">
                <img id="cert_bride_photo" alt="Bride" style="display:none"/>
                <span id="cert_bride_ph">Bride<br/>Photo</span>
              </div>
              <div class="cert-side-label">वधू / BRIDE</div>
            </div>
          </div>

          <!-- Place / Date row -->
          <div class="cert-info-row">
            <div><b>PLACE OF MARRIAGE:</b> <span id="cert_place" style="border-bottom:1px dotted #555; padding:0 8px; font-weight:600;"></span></div>
            <div><b>DATE OF MARRIAGE:</b> <span id="cert_date" style="border-bottom:1px dotted #555; padding:0 8px; font-weight:600;"></span></div>
          </div>

          <!-- Certificate No -->
          <div class="cert-ref">
            <b>Certificate No.:</b> <span class="cert-ref-line" id="cert_ref"></span>
          </div>

          <!-- Statement -->
          <div class="cert-statement">
            It is certified that the marriage between the following persons have been solemnised according to <b>Hindu Vedic rites and customs</b>.
          </div>

          <!-- Main Table -->
          <table class="cert-table">
            <thead>
              <tr>
                <th></th>
                <th>NAME</th>
                <th>FATHER'S NAME</th>
                <th>PERMANENT ADDRESS</th>
                <th>AGE / DOB</th>
                <th>NATIONALITY</th>
                <th>STATUS</th>
                <th>SIGNATURES</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td class="row-label">BRIDE</td>
                <td id="t_b_name" style="font-weight:600;"></td>
                <td id="t_b_father"></td>
                <td id="t_b_address"></td>
                <td id="t_b_age" style="text-align:center;"></td>
                <td style="text-align:center;">Indian</td>
                <td style="text-align:center;">Unmarried</td>
                <td style="min-width:60px;"></td>
              </tr>
              <tr>
                <td class="row-label">BRIDE-GROOM</td>
                <td id="t_g_name" style="font-weight:600;"></td>
                <td id="t_g_father"></td>
                <td id="t_g_address"></td>
                <td id="t_g_age" style="text-align:center;"></td>
                <td style="text-align:center;">Indian</td>
                <td style="text-align:center;">Unmarried</td>
                <td style="min-width:60px;"></td>
              </tr>
            </tbody>
          </table>

          <!-- Witnesses -->
          <div class="cert-witnesses">
            <div class="witness-block">
              <h4>Witness 1</h4>
              <p><b>Signature:</b> <span class="line"></span></p>
              <p><b>Name:</b> <span class="line" id="t_w1_name"></span></p>
              <p><b>S/o, D/o, W/o:</b> <span class="line" id="t_w1_father"></span></p>
              <p><b>R/o:</b> <span class="line" id="t_w1_address"></span></p>
            </div>
            <div class="witness-block">
              <h4>Witness 2</h4>
              <p><b>Signature:</b> <span class="line"></span></p>
              <p><b>Name:</b> <span class="line" id="t_w2_name"></span></p>
              <p><b>S/o, D/o, W/o:</b> <span class="line" id="t_w2_father"></span></p>
              <p><b>R/o:</b> <span class="line" id="t_w2_address"></span></p>
            </div>
          </div>

          <!-- Warning -->
          <div class="cert-warning">
            <b>WARNING:-</b> Submission of false information or forged documents will make this certificate invalid. We will keep the documents certificate record's here for 1 year only. You should get your marriage registered as soon as possible in concerned jurisdiction. The authenticity of this document should be verified at https://aryasamajvivahmandir.com
          </div>

          <!-- Bottom row: Pandit signature + (Bottom corner stamp SVG) -->
          <div class="cert-bottom-row">
            <div class="cert-pandit-sign">
              <div class="sign-line-top"></div>
              SIGNATURE OF THE PANDIT/PRIEST<br/>
              WHO PERFORMED THE CEREMONY
            </div>
            <div class="cert-bottom-stamp-wrap">
              <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg" width="120" height="120" style="opacity:0.92;">
                <defs>
                  <path id="stampBottomArc" d="M 100,100 m -80,0 a 80,80 0 1,1 160,0" fill="none"/>
                </defs>
                <circle cx="100" cy="100" r="88" fill="none" stroke="#1a237e" stroke-width="2.5"/>
                <circle cx="100" cy="100" r="64" fill="none" stroke="#1a237e" stroke-width="1.8"/>
                <text font-size="14" font-weight="bold" fill="#1a237e" font-family="Arial, sans-serif" letter-spacing="0.5">
                  <textPath href="#stampBottomArc" startOffset="50%" text-anchor="middle">ARYA SAMAJ MARRIAGE TRUST (REGD.)</textPath>
                </text>
                <text x="100" y="88" text-anchor="middle" font-size="13" font-weight="bold" fill="#1a237e">Regd. No.</text>
                <text x="100" y="110" text-anchor="middle" font-size="17" font-weight="bold" fill="#1a237e">1212/22</text>
                <text x="100" y="130" text-anchor="middle" font-size="14" font-weight="bold" fill="#1a237e">DELHI</text>
              </svg>
            </div>
          </div>

          <!-- Group Wedding Photo with witnesses -->
          <div class="cert-group-photo-wrap" id="cert_group_wrap">
            <div class="cert-group-photo">
              <img id="cert_group_photo" alt="Group" style="display:none"/>
              <span id="cert_group_ph" style="font-size:11px;color:#999;">Group Wedding Photo (with Witnesses)</span>
            </div>
            <div class="cert-group-label">📸 Group Wedding Photograph (with Witnesses)</div>
          </div>

          <!-- Digital signature style verification block + QR -->
          <div class="cert-digisign">
            <div class="cert-digisign-icon">✓</div>
            <div class="cert-digisign-text">
              <div><b>Digitally Verified by Arya Samaj Marriage Trust</b></div>
              <div style="font-size:10px;color:#555;">Signed on: <span id="cert_digisign_dt"></span></div>
              <div style="font-size:10px;color:#555;">Reason: Marriage Solemnised | Location: Delhi, India</div>
              <div style="font-size:10px;color:#555;">Scan QR code → Verify online</div>
            </div>
            <div class="cert-qr-block">
              <div class="qr-wrap" id="cert_qr"></div>
              <div class="qr-label">SCAN TO<br/>VERIFY</div>
            </div>
          </div>

          <div style="text-align:center; font-size:10px; color:#777; margin-top:8px; padding-top:6px; border-top:1px dashed #ccc;">
            <b>Certificate ID:</b> <span id="cv_cert_id"></span> &nbsp;|&nbsp;
            <b>Issued:</b> <span id="cert_issue_date"></span>
          </div>

        </div>

        <div class="swastika-v"></div>
      </div>

      <!-- BOTTOM swastika border -->
      <div class="swastika-h"></div>
    </div>
  </div>

  <!-- ========= SETTINGS ========= -->
  <div id="settingsPage" class="container hidden">
    <h1 class="page-title">Settings</h1>
    <p class="page-subtitle">Configure password and verification URL</p>

    <div class="form-card" style="max-width: 600px;">
      <div class="form-section-title">🔐 Change Password</div>
      <div id="settingsAlert"></div>
      <form onsubmit="return changePassword(event)">
        <div class="form-group"><label>Current Password</label><input type="password" id="curPass" required/></div>
        <div class="form-group"><label>New Password</label><input type="password" id="newPass" required minlength="4"/></div>
        <div class="form-group"><label>Confirm New Password</label><input type="password" id="confPass" required minlength="4"/></div>
        <div class="form-actions">
          <button type="button" class="btn-secondary" onclick="showDashboard()">Cancel</button>
          <button type="submit" class="btn-primary" style="width:auto; padding: 12px 22px;">Update Password</button>
        </div>
      </form>
    </div>

    <div class="form-card" style="max-width: 600px;">
      <div class="form-section-title">🌐 Public Verification URL (for QR code)</div>
      <div id="urlAlert"></div>
      <p style="font-size:13px; color:#555; margin-bottom:12px;">
        Yahan apni GitHub Pages ya hosted website ka URL daalo. QR code is URL ko use karega.<br/>
        Example: <code>https://yourname.github.io/marriage-cert/</code>
      </p>
      <form onsubmit="return savePublicURL(event)">
        <div class="form-group">
          <label>Public Verification URL</label>
          <input type="url" id="publicURL" placeholder="https://yourname.github.io/marriage-cert/"/>
          <div class="small-note">Empty rakhne par current page URL use hoga.</div>
        </div>
        <div class="form-actions">
          <button type="submit" class="btn-primary" style="width:auto; padding: 12px 22px;">💾 Save URL</button>
        </div>
      </form>
    </div>
  </div>


</div>

<!-- ========= VERIFICATION PAGE (shown when URL has #verify=... — works WITHOUT login) ========= -->
<div id="verifyPage" class="verify-wrap hidden">
  <div class="verify-card">
    <div id="verifyContent"></div>
  </div>
</div>

<script>
/* ============================================
   STORAGE KEYS
   ============================================ */
const K_AUTH = 'cmc_auth_v1';
const K_USER = 'cmc_loggedUser';
const K_CERTS = 'cmc_certificates_v1';
const K_PROFILES = 'cmc_profiles_v1';   // keyed by Aadhaar

/* ============================================
   INITIAL SETUP
   ============================================ */
(function init() {
  if (!localStorage.getItem(K_AUTH)) {
    localStorage.setItem(K_AUTH, JSON.stringify({ username: 'admin', password: 'admin123' }));
  }
  if (!localStorage.getItem(K_CERTS)) localStorage.setItem(K_CERTS, '[]');
  if (!localStorage.getItem(K_PROFILES)) localStorage.setItem(K_PROFILES, '{}');

  // Check for verification mode FIRST (works without login)
  if (window.location.hash && window.location.hash.startsWith('#verify=')) {
    showVerifyPage();
    return;
  }

  if (localStorage.getItem(K_USER)) {
    showApp();
  }
})();

// Listen for hash changes (e.g. opening from QR while logged in)
window.addEventListener('hashchange', () => {
  if (window.location.hash.startsWith('#verify=')) {
    showVerifyPage();
  }
});

/* ============================================
   AUTH
   ============================================ */
function doLogin(e) {
  e.preventDefault();
  const u = document.getElementById('loginUser').value.trim();
  const p = document.getElementById('loginPass').value;
  const auth = JSON.parse(localStorage.getItem(K_AUTH));
  if (u === auth.username && p === auth.password) {
    localStorage.setItem(K_USER, u);
    showApp();
  } else {
    document.getElementById('loginAlert').innerHTML =
      '<div class="alert-error">❌ Invalid username or password</div>';
  }
  return false;
}

function doLogout() {
  if (!confirm('Are you sure you want to logout?')) return;
  localStorage.removeItem(K_USER);
  document.getElementById('appShell').classList.add('hidden');
  document.getElementById('loginPage').classList.remove('hidden');
  document.getElementById('loginForm').reset();
  document.getElementById('loginAlert').innerHTML = '';
}

function showApp() {
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('appShell').classList.remove('hidden');
  document.getElementById('navUser').textContent = 'Welcome, ' + localStorage.getItem(K_USER);
  showDashboard();
}

function changePassword(e) {
  e.preventDefault();
  const cur = document.getElementById('curPass').value;
  const nw = document.getElementById('newPass').value;
  const cf = document.getElementById('confPass').value;
  const auth = JSON.parse(localStorage.getItem(K_AUTH));
  const alertBox = document.getElementById('settingsAlert');
  if (cur !== auth.password) {
    alertBox.innerHTML = '<div class="alert-error">❌ Current password is incorrect</div>';
    return false;
  }
  if (nw !== cf) {
    alertBox.innerHTML = '<div class="alert-error">❌ New passwords do not match</div>';
    return false;
  }
  auth.password = nw;
  localStorage.setItem(K_AUTH, JSON.stringify(auth));
  alertBox.innerHTML = '<div class="alert-success">✅ Password updated successfully!</div>';
  document.getElementById('curPass').value = '';
  document.getElementById('newPass').value = '';
  document.getElementById('confPass').value = '';
  return false;
}

/* ============================================
   PAGE NAVIGATION
   ============================================ */
function hideAllPages() {
  ['dashboardPage','formPage','certPage','settingsPage'].forEach(id =>
    document.getElementById(id).classList.add('hidden'));
}
function showDashboard() { hideAllPages(); document.getElementById('dashboardPage').classList.remove('hidden'); loadDashboard(); window.scrollTo(0,0); }
function showForm() {
  hideAllPages();
  document.getElementById('formPage').classList.remove('hidden');
  document.getElementById('appForm').reset();
  ['g_photo_preview','b_photo_preview','m_photo_preview'].forEach(id => {
    const el = document.getElementById(id); el.style.display='none'; el.src='';
  });
  ['g_age_disp','b_age_disp'].forEach(id => document.getElementById(id).textContent='');
  document.getElementById('formAlert').innerHTML='';
  // Reset edit mode (we're creating a new certificate)
  editingCertNo = null;
  editingPhotos = {};
  const sb = document.querySelector('#appForm button[type="submit"]');
  if (sb) sb.innerHTML = '📜 Generate Certificate';
  // Reset state defaults
  const stateEl = document.getElementById('m_state'); if (stateEl && !stateEl.value) stateEl.value = 'Delhi';
  const officerEl = document.getElementById('m_officer'); if (officerEl && !officerEl.value) officerEl.value = 'Pandit Ji';
  window.scrollTo(0,0);
}
function showSettings()  { hideAllPages(); document.getElementById('settingsPage').classList.remove('hidden'); document.getElementById('settingsAlert').innerHTML=''; window.scrollTo(0,0); }

/* ============================================
   DASHBOARD
   ============================================ */
function loadDashboard() {
  const certs = JSON.parse(localStorage.getItem(K_CERTS) || '[]');
  const profs = JSON.parse(localStorage.getItem(K_PROFILES) || '{}');
  document.getElementById('statTotal').textContent = certs.length;
  document.getElementById('statProfiles').textContent = Object.keys(profs).length;
  const now = new Date();
  const monthCount = certs.filter(c => {
    const d = new Date(c.createdAt);
    return d.getFullYear() === now.getFullYear() && d.getMonth() === now.getMonth();
  }).length;
  document.getElementById('statMonth').textContent = monthCount;

  const tbody = document.getElementById('recordsBody');
  if (certs.length === 0) {
    tbody.innerHTML = '<tr><td colspan="6" class="empty-row">No certificates yet. Click "New Certificate Application" to start.</td></tr>';
    return;
  }
  tbody.innerHTML = certs.slice().reverse().map(c => `
    <tr>
      <td><b>${escapeHTML(c.certNo)}</b></td>
      <td>${escapeHTML(c.groom.name)}</td>
      <td>${escapeHTML(c.bride.name)}</td>
      <td>${formatDate(c.marriage.date)}</td>
      <td>${formatDate(c.createdAt)}</td>
      <td class="table-actions">
        <button class="btn-view" onclick="viewCert('${c.certNo}')">👁 View</button>
        <button class="btn-edit" onclick="editCert('${c.certNo}')">✏️ Edit</button>
        <button class="btn-delete" onclick="deleteCert('${c.certNo}')">🗑 Delete</button>
      </td>
    </tr>
  `).join('');
}

function deleteCert(certNo) {
  if (!confirm('Delete this certificate? This cannot be undone.')) return;
  let certs = JSON.parse(localStorage.getItem(K_CERTS) || '[]');
  certs = certs.filter(c => c.certNo !== certNo);
  localStorage.setItem(K_CERTS, JSON.stringify(certs));
  loadDashboard();
}

function viewCert(certNo) {
  const certs = JSON.parse(localStorage.getItem(K_CERTS) || '[]');
  const c = certs.find(x => x.certNo === certNo);
  if (!c) return alert('Certificate not found');
  renderCertificate(c);
  hideAllPages();
  document.getElementById('certPage').classList.remove('hidden');
  window.scrollTo(0,0);
}

/* ============================================
   PROFILE FETCH (Aadhaar based local lookup)
   ============================================ */
function fetchProfile(prefix) {
  const aadhaar = document.getElementById(prefix + '_aadhaar').value.trim();
  if (!/^\d{12}$/.test(aadhaar)) {
    alert('Please enter a valid 12-digit Aadhaar number.');
    return;
  }
  const profs = JSON.parse(localStorage.getItem(K_PROFILES) || '{}');
  const p = profs[aadhaar];
  if (!p) {
    alert('No saved profile for this Aadhaar. Please fill the details manually — they will be saved for future auto-fill.');
    return;
  }
  // Map fields based on prefix
  const mapping = {
    g: ['name','father','mother','dob','religion','occupation','mobile','address'],
    b: ['name','father','mother','dob','religion','occupation','mobile','address'],
    w1: ['name','father','mobile','address','relation'],
    w2: ['name','father','mobile','address','relation']
  };
  const fields = mapping[prefix];
  fields.forEach(f => {
    const el = document.getElementById(prefix + '_' + f);
    if (el && p[f] !== undefined) el.value = p[f];
  });
  if (prefix === 'g' || prefix === 'b') calcAge(prefix);
  alert('✅ Profile auto-filled from saved records.');
}

function saveProfileFromForm(prefix, fields) {
  const aadhaar = document.getElementById(prefix + '_aadhaar').value.trim();
  if (!/^\d{12}$/.test(aadhaar)) return;
  const profs = JSON.parse(localStorage.getItem(K_PROFILES) || '{}');
  const obj = {};
  fields.forEach(f => {
    const el = document.getElementById(prefix + '_' + f);
    if (el) obj[f] = el.value;
  });
  profs[aadhaar] = obj;
  localStorage.setItem(K_PROFILES, JSON.stringify(profs));
}

/* ============================================
   AGE CALCULATION & VALIDATION
   ============================================ */
function calculateAge(dobStr) {
  if (!dobStr) return null;
  const dob = new Date(dobStr);
  const today = new Date();
  let age = today.getFullYear() - dob.getFullYear();
  const m = today.getMonth() - dob.getMonth();
  if (m < 0 || (m === 0 && today.getDate() < dob.getDate())) age--;
  return age;
}

function calcAge(prefix) {
  const dob = document.getElementById(prefix + '_dob').value;
  const disp = document.getElementById(prefix + '_age_disp');
  if (!dob) { disp.textContent = ''; return; }
  const age = calculateAge(dob);
  const minAge = prefix === 'g' ? 21 : 18;
  if (age < minAge) {
    disp.className = 'age-display error';
    disp.textContent = `⚠ Age: ${age} years — Must be ${minAge}+ years`;
  } else {
    disp.className = 'age-display ok';
    disp.textContent = `✓ Age: ${age} years`;
  }
}

/* ============================================
   PHOTO PREVIEW
   ============================================ */
function previewPhoto(inputId, previewId) {
  const file = document.getElementById(inputId).files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    const img = document.getElementById(previewId);
    img.src = e.target.result;
    img.style.display = 'inline-block';
  };
  reader.readAsDataURL(file);
}

function readFileAsDataURL(inputId) {
  return new Promise(resolve => {
    const file = document.getElementById(inputId).files[0];
    if (!file) return resolve(null);
    const reader = new FileReader();
    reader.onload = e => resolve(e.target.result);
    reader.readAsDataURL(file);
  });
}

/* ============================================
   GENERATE / UPDATE CERTIFICATE
   ============================================ */
let editingCertNo = null;   // when set, we're editing an existing cert
let editingPhotos = {};     // preserve old photos when editing if no new file uploaded

async function generateCertificate(e) {
  e.preventDefault();
  const alertBox = document.getElementById('formAlert');
  alertBox.innerHTML = '';

  // Gather basic data
  const gDob = document.getElementById('g_dob').value;
  const bDob = document.getElementById('b_dob').value;
  const gAge = calculateAge(gDob);
  const bAge = calculateAge(bDob);

  const errors = [];
  if (gAge === null || gAge < 21) errors.push(`❌ Groom (${document.getElementById('g_name').value || 'Groom'}) age is ${gAge !== null ? gAge : '?'} years. Minimum age required: 21 years.`);
  if (bAge === null || bAge < 18) errors.push(`❌ Bride (${document.getElementById('b_name').value || 'Bride'}) age is ${bAge !== null ? bAge : '?'} years. Minimum age required: 18 years.`);

  // Aadhaar uniqueness check
  const aadhaars = ['g_aadhaar','b_aadhaar','w1_aadhaar','w2_aadhaar'].map(id => document.getElementById(id).value.trim());
  for (let a of aadhaars) {
    if (!/^\d{12}$/.test(a)) errors.push('❌ All Aadhaar numbers must be exactly 12 digits.');
  }
  const uniq = new Set(aadhaars);
  if (uniq.size !== aadhaars.length) errors.push('❌ Aadhaar numbers must be unique for groom, bride, and both witnesses.');

  if (errors.length) {
    alertBox.innerHTML = '<div class="alert-error">' + errors.join('<br/>') + '<br/><br/><b>Certificate cannot be generated.</b></div>';
    window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' });
    return false;
  }

  // Read photos (preserve old when editing if user did not upload new ones)
  const gPhoto = (await readFileAsDataURL('g_photo')) || editingPhotos.groom || null;
  const bPhoto = (await readFileAsDataURL('b_photo')) || editingPhotos.bride || null;
  const mPhoto = (await readFileAsDataURL('m_photo')) || editingPhotos.marriage || null;

  // Save profiles for future auto-fill
  saveProfileFromForm('g', ['name','father','mother','dob','religion','occupation','mobile','address']);
  saveProfileFromForm('b', ['name','father','mother','dob','religion','occupation','mobile','address']);
  saveProfileFromForm('w1', ['name','father','mobile','address','relation']);
  saveProfileFromForm('w2', ['name','father','mobile','address','relation']);

  const marriageDate = document.getElementById('m_date').value;
  const certs = JSON.parse(localStorage.getItem(K_CERTS) || '[]');

  let certData;
  if (editingCertNo) {
    // UPDATE existing certificate (keep certNo and createdAt)
    const idx = certs.findIndex(x => x.certNo === editingCertNo);
    const old = certs[idx] || {};
    certData = {
      certNo: editingCertNo,
      createdAt: old.createdAt || new Date().toISOString(),
      updatedAt: new Date().toISOString(),
      groom: collectPerson('g', gAge, gPhoto),
      bride: collectPerson('b', bAge, bPhoto),
      witness1: collectWitness('w1'),
      witness2: collectWitness('w2'),
      marriage: {
        date: marriageDate,
        place: document.getElementById('m_place').value,
        officer: document.getElementById('m_officer').value,
        state: document.getElementById('m_state').value,
        act: document.getElementById('m_act').value,
        photo: mPhoto
      }
    };
    if (idx >= 0) certs[idx] = certData; else certs.push(certData);
  } else {
    // CREATE new certificate
    const certNo = generateCertNo(marriageDate);
    certData = {
      certNo,
      createdAt: new Date().toISOString(),
      groom: collectPerson('g', gAge, gPhoto),
      bride: collectPerson('b', bAge, bPhoto),
      witness1: collectWitness('w1'),
      witness2: collectWitness('w2'),
      marriage: {
        date: marriageDate,
        place: document.getElementById('m_place').value,
        officer: document.getElementById('m_officer').value,
        state: document.getElementById('m_state').value,
        act: document.getElementById('m_act').value,
        photo: mPhoto
      }
    };
    certs.push(certData);
  }

  localStorage.setItem(K_CERTS, JSON.stringify(certs));
  editingCertNo = null;
  editingPhotos = {};

  renderCertificate(certData);
  hideAllPages();
  document.getElementById('certPage').classList.remove('hidden');
  window.scrollTo(0,0);
  return false;
}

/* ============================================
   EDIT EXISTING CERTIFICATE
   ============================================ */
function editCert(certNo) {
  const certs = JSON.parse(localStorage.getItem(K_CERTS) || '[]');
  const c = certs.find(x => x.certNo === certNo);
  if (!c) return alert('Certificate not found');

  // Open form fresh, then populate
  hideAllPages();
  document.getElementById('formPage').classList.remove('hidden');
  document.getElementById('appForm').reset();
  document.getElementById('formAlert').innerHTML =
    '<div class="alert-success">✏️ Editing certificate <b>' + c.certNo + '</b> — make changes and click "Update Certificate" to save.</div>';

  // Person blocks
  const setVal = (id, v) => { const el = document.getElementById(id); if (el) el.value = v || ''; };
  ['name','father','mother','dob','religion','occupation','mobile','aadhaar','address'].forEach(f => setVal('g_'+f, c.groom[f]));
  ['name','father','mother','dob','religion','occupation','mobile','aadhaar','address'].forEach(f => setVal('b_'+f, c.bride[f]));
  ['name','father','mobile','aadhaar','address','relation'].forEach(f => setVal('w1_'+f, c.witness1[f]));
  ['name','father','mobile','aadhaar','address','relation'].forEach(f => setVal('w2_'+f, c.witness2[f]));
  setVal('m_date', c.marriage.date);
  setVal('m_place', c.marriage.place);
  setVal('m_officer', c.marriage.officer);
  setVal('m_state', c.marriage.state);
  setVal('m_act', c.marriage.act);

  // Show existing photos as preview
  showExistingPhoto('g_photo_preview', c.groom.photo);
  showExistingPhoto('b_photo_preview', c.bride.photo);
  showExistingPhoto('m_photo_preview', c.marriage.photo);

  // Display ages
  calcAge('g'); calcAge('b');

  // Mark editing state
  editingCertNo = certNo;
  editingPhotos = {
    groom: c.groom.photo,
    bride: c.bride.photo,
    marriage: c.marriage.photo
  };

  // Change submit button label
  const sb = document.querySelector('#appForm button[type="submit"]');
  if (sb) sb.innerHTML = '💾 Update Certificate';
  window.scrollTo(0,0);
}

function showExistingPhoto(previewId, src) {
  const img = document.getElementById(previewId);
  if (!img) return;
  if (src) { img.src = src; img.style.display = 'inline-block'; }
  else { img.style.display = 'none'; img.src = ''; }
}

function collectPerson(p, age, photo) {
  return {
    name: document.getElementById(p+'_name').value,
    father: document.getElementById(p+'_father').value,
    mother: document.getElementById(p+'_mother').value,
    dob: document.getElementById(p+'_dob').value,
    age,
    religion: document.getElementById(p+'_religion').value,
    occupation: document.getElementById(p+'_occupation').value,
    mobile: document.getElementById(p+'_mobile').value,
    aadhaar: document.getElementById(p+'_aadhaar').value,
    address: document.getElementById(p+'_address').value,
    photo
  };
}

function collectWitness(p) {
  return {
    name: document.getElementById(p+'_name').value,
    father: document.getElementById(p+'_father').value,
    mobile: document.getElementById(p+'_mobile').value,
    aadhaar: document.getElementById(p+'_aadhaar').value,
    address: document.getElementById(p+'_address').value,
    relation: document.getElementById(p+'_relation').value
  };
}

function generateCertNo(marriageDateStr) {
  // Format: M{DD}{MM}{SEQ}/{YY}
  // DD = marriage date day, MM = marriage month, SEQ = running counter, YY = year (last 2 digits)
  // Example: M3004515/26 → Marriage on 30 April, sequence #515, year 2026
  const d = marriageDateStr ? new Date(marriageDateStr) : new Date();
  const dd = String(d.getDate()).padStart(2, '0');
  const mm = String(d.getMonth() + 1).padStart(2, '0');
  const yy = String(d.getFullYear()).slice(-2);

  // Sequence counter (starts from 514, first cert becomes 515)
  let seq = parseInt(localStorage.getItem('cmc_seq') || '514', 10);
  seq++;
  localStorage.setItem('cmc_seq', String(seq));
  const seqStr = String(seq).padStart(3, '0');

  return `M${dd}${mm}${seqStr}/${yy}`;
}

/* ============================================
   RENDER CERTIFICATE
   ============================================ */
function renderCertificate(c) {
  const $ = id => document.getElementById(id);

  // Top meta
  $('cert_place').textContent = (c.marriage.place || '') + (c.marriage.state ? ', ' + c.marriage.state : '');
  $('cert_date').textContent = formatDate(c.marriage.date);
  $('cert_ref').textContent = c.certNo;
  $('cert_issue_date').textContent = formatDate(c.createdAt);
  $('cv_cert_id').textContent = c.certNo;

  // Top side photos: GROOM (left) | BRIDE (right)
  setPhoto('cert_groom_photo', 'cert_groom_ph', c.groom.photo);
  setPhoto('cert_bride_photo', 'cert_bride_ph', c.bride.photo);

  // Group / wedding photo at bottom (with witnesses)
  setPhoto('cert_group_photo', 'cert_group_ph', c.marriage.photo);
  // Hide the group photo block entirely if no photo
  const groupWrap = $('cert_group_wrap');
  if (groupWrap) groupWrap.style.display = c.marriage.photo ? 'block' : 'none';

  // Bride row in table
  setMiniPhoto('t_b_photo', c.bride.photo);
  $('t_b_name').textContent = c.bride.name;
  $('t_b_father').textContent = c.bride.father;
  $('t_b_address').textContent = c.bride.address;
  $('t_b_age').textContent = c.bride.age + ' / ' + formatDate(c.bride.dob);

  // Groom row in table
  setMiniPhoto('t_g_photo', c.groom.photo);
  $('t_g_name').textContent = c.groom.name;
  $('t_g_father').textContent = c.groom.father;
  $('t_g_address').textContent = c.groom.address;
  $('t_g_age').textContent = c.groom.age + ' / ' + formatDate(c.groom.dob);

  // Witnesses
  $('t_w1_name').textContent = c.witness1.name;
  $('t_w1_father').textContent = c.witness1.father;
  $('t_w1_address').textContent = c.witness1.address;
  $('t_w2_name').textContent = c.witness2.name;
  $('t_w2_father').textContent = c.witness2.father;
  $('t_w2_address').textContent = c.witness2.address;

  // Pandit signature name
  $('cv_officer').textContent = c.marriage.officer || '';

  // Digital signature timestamp (Arya Samaj branded)
  const signDt = c.updatedAt || c.createdAt;
  const dt = new Date(signDt);
  const dtStr = dt.toLocaleString('en-IN', {
    day: '2-digit', month: 'short', year: 'numeric',
    hour: '2-digit', minute: '2-digit', hour12: true
  }) + ' IST';
  $('cert_digisign_dt').textContent = dtStr;
}

function setMiniPhoto(imgId, src) {
  const img = document.getElementById(imgId);
  if (!img) return;
  if (src) { img.src = src; img.style.display = 'inline-block'; }
  else { img.style.display = 'none'; img.src = ''; }
}

function setPhoto(imgId, phId, src) {
  const img = document.getElementById(imgId);
  const ph = document.getElementById(phId);
  if (src) {
    img.src = src;
    img.style.display = 'block';
    ph.style.display = 'none';
  } else {
    img.style.display = 'none';
    ph.style.display = 'block';
  }
}

/* ============================================
   PDF DOWNLOAD
   ============================================ */
async function downloadPDF() {
  const cert = document.getElementById('certificate');
  const certNoEl = document.getElementById('cert_ref') || document.getElementById('cv_cert_id');
  const certNo = (certNoEl ? certNoEl.textContent : 'certificate').replace(/[\/\\]/g, '_');
  try {
    const canvas = await html2canvas(cert, { scale: 2, useCORS: true, backgroundColor: '#fff' });
    const imgData = canvas.toDataURL('image/jpeg', 0.95);
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF({ unit: 'mm', format: 'a4', orientation: 'portrait' });
    const pageW = pdf.internal.pageSize.getWidth();
    const pageH = pdf.internal.pageSize.getHeight();
    const imgW = pageW - 10;
    const imgH = (canvas.height * imgW) / canvas.width;
    let y = 5;
    if (imgH < pageH - 10) {
      pdf.addImage(imgData, 'JPEG', 5, y, imgW, imgH);
    } else {
      // Multi-page handling
      let remaining = imgH;
      let position = 5;
      while (remaining > 0) {
        pdf.addImage(imgData, 'JPEG', 5, position, imgW, imgH);
        remaining -= (pageH - 10);
        position -= (pageH - 10);
        if (remaining > 0) pdf.addPage();
      }
    }
    pdf.save(`Marriage_Certificate_${certNo}.pdf`);
  } catch (err) {
    console.error(err);
    alert('Error generating PDF: ' + err.message);
  }
}

/* ============================================
   HELPERS
   ============================================ */
function formatDate(d) {
  if (!d) return '-';
  const dt = new Date(d);
  if (isNaN(dt)) return d;
  return dt.toLocaleDateString('en-IN', { day: '2-digit', month: 'long', year: 'numeric' });
}
function escapeHTML(s) { return String(s||'').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c])); }
function maskAadhaar(a) { if (!a) return '-'; return 'XXXX-XXXX-' + a.slice(-4); }
</script>
</body>
</html>
