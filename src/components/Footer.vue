<template>
  <footer class="footer">
    <div class="footer-content">
      <div class="footer-section">
        <h3>{{ t('footer.platform.title') }}</h3>
        <ul>
          <li><a href="#">{{ t('footer.platform.merchant') }}</a></li>
          <li><a href="#">{{ t('footer.platform.open') }}</a></li>
          <li><a href="#">{{ t('footer.platform.join') }}</a></li>
        </ul>
      </div>

      <div class="footer-section">
        <h3>{{ t('footer.legal.title') }}</h3>
        <ul>
          <li><a href="#">{{ t('footer.legal.privacy') }}</a></li>
          <li><a href="#">{{ t('footer.legal.terms') }}</a></li>
          <li><a href="#">{{ t('footer.legal.copyright') }}</a></li>
          <li><a href="#">{{ t('footer.legal.disclaimer') }}</a></li>
          <li><a href="#">{{ t('footer.legal.report') }}</a></li>
        </ul>
      </div>

      <div class="footer-section">
        <h3>{{ t('footer.follow.title') }}</h3>
        <ul class="social-links">
          <li>
            <a href="#">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
                <path d="M18.5 2h-13C3.57 2 2 3.57 2 5.5v13C2 20.43 3.57 22 5.5 22h13c1.93 0 3.5-1.57 3.5-3.5v-13C22 3.57 20.43 2 18.5 2z"/>
              </svg>
              {{ t('footer.follow.wechat') }}
            </a>
          </li>
          <li>
            <a href="#">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
                <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"/>
              </svg>
              {{ t('footer.follow.miniapp') }}
            </a>
          </li>
          <li>
            <a href="#">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
                <path d="M22 12c0-5.52-4.48-10-10-10S2 6.48 2 12c0 4.84 3.44 8.87 8 9.8V15H8v-3h2V9.5C10 7.57 11.57 6 13.5 6H16v3h-2c-.55 0-1 .45-1 1v2h3v3h-3v6.95c5.05-.5 9-4.76 9-9.95z"/>
              </svg>
              {{ t('footer.follow.weibo') }}
            </a>
          </li>
        </ul>
      </div>

      <div class="footer-section download-section">
        <h3>{{ t('footer.download.title') }}</h3>
        <div class="qr-code">
          <div class="qr-placeholder">
            <svg width="80" height="80" viewBox="0 0 100 100" fill="#333">
              <rect width="100" height="100" fill="#fff"/>
              <rect x="10" y="10" width="15" height="15" fill="#000"/>
              <rect x="75" y="10" width="15" height="15" fill="#000"/>
              <rect x="10" y="75" width="15" height="15" fill="#000"/>
              <rect x="45" y="45" width="10" height="10" fill="#000"/>
            </svg>
          </div>
          <p class="qr-text">{{ t('footer.download.scanQR') }}</p>
        </div>
        <div class="download-links">
          <a href="#" class="download-btn">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
              <path d="M18.71 19.5c-.83 1.24-1.71 2.45-3.05 2.47-1.34.03-1.77-.79-3.29-.79-1.53 0-2 .77-3.27.82-1.31.05-2.3-1.32-3.14-2.53C4.25 17 2.94 12.45 4.7 9.39c.87-1.52 2.43-2.48 4.12-2.51 1.28-.02 2.5.87 3.29.87.78 0 2.26-1.07 3.81-.91.65.03 2.47.26 3.64 1.98-.09.06-2.17 1.28-2.15 3.81.03 3.02 2.65 4.03 2.68 4.04-.03.07-.42 1.44-1.38 2.83M13 3.5c.73-.83 1.94-1.46 2.94-1.5.13 1.17-.34 2.35-1.04 3.19-.69.85-1.83 1.51-2.95 1.42-.15-1.15.41-2.35 1.05-3.11z"/>
            </svg>
            {{ t('footer.download.appStore') }}
          </a>
          <a href="#" class="download-btn">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
              <path d="M3 20.5v-17c0-.59.34-1.11.84-1.35L13.69 12l-9.85 9.85c-.5-.24-.84-.76-.84-1.35zm13.81-5.38L6.05 21.34l8.49-4.86 2.27-1.36zm2.27-1.36l-2.27-1.36-8.49-4.86 10.76 6.22zM13.69 12L3.84 2.15C4.34 1.91 4.91 1.91 5.41 2.15L20.5 11.5l-6.81 4z"/>
            </svg>
            {{ t('footer.download.android') }}
          </a>
        </div>
      </div>
    </div>

    <div class="footer-bottom">
      <div class="footer-info">
        <p>{{ t('footer.company.name') }}</p>
        <p class="copyright">© 2024 LittleMei. {{ t('footer.company.rights') }}</p>
        <p class="contact">
          {{ t('footer.company.email') }}: contact@littlemei.com | 
          {{ t('footer.company.phone') }}: 400-888-8888
        </p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { useI18n } from 'vue-i18n'

const { t } = useI18n()
</script>

<style scoped>
.footer {
  background: #1a1a1a;
  color: #ccc;
  padding: 60px 20px 30px;
  margin-top: 100px;
}

.footer-content {
  max-width: 1400px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 40px;
  padding-bottom: 40px;
  border-bottom: 1px solid #333;
}

.footer-section h3 {
  color: #fff;
  font-size: 16px;
  font-weight: 700;
  margin-bottom: 20px;
  letter-spacing: 0.5px;
}

.footer-section ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.footer-section li {
  margin-bottom: 12px;
}

.footer-section a {
  color: #999;
  text-decoration: none;
  font-size: 14px;
  transition: color 0.3s;
  display: flex;
  align-items: center;
  gap: 8px;
}

.footer-section a:hover {
  color: #fff;
}

.social-links svg {
  opacity: 0.7;
  transition: opacity 0.3s;
}

.social-links a:hover svg {
  opacity: 1;
}

.download-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.qr-code {
  margin: 20px 0;
}

.qr-placeholder {
  width: 120px;
  height: 120px;
  background: #fff;
  border-radius: 8px;
  padding: 10px;
  margin: 0 auto 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.qr-text {
  font-size: 12px;
  color: #999;
}

.download-links {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 100%;
  max-width: 220px;
}

.download-btn {
  padding: 12px 20px;
  background: #2a2a2a;
  color: #fff !important;
  border-radius: 8px;
  font-size: 13px;
  font-weight: 600;
  transition: all 0.3s;
  border: 1px solid #3a3a3a;
}

.download-btn:hover {
  background: #3a3a3a;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
}

.footer-bottom {
  max-width: 1400px;
  margin: 0 auto;
  padding-top: 30px;
}

.footer-info {
  text-align: center;
}

.footer-info p {
  color: #666;
  font-size: 13px;
  margin: 8px 0;
  line-height: 1.6;
}

.footer-info .copyright {
  font-weight: 600;
  color: #888;
}

.contact {
  margin-top: 15px;
  font-size: 12px !important;
}

@media (max-width: 768px) {
  .footer {
    padding: 40px 15px 20px;
    margin-top: 60px;
  }

  .footer-content {
    grid-template-columns: 1fr;
    gap: 30px;
  }

  .footer-section {
    text-align: center;
  }

  .footer-section a {
    justify-content: center;
  }

  .download-section {
    padding-top: 20px;
    border-top: 1px solid #333;
  }

  .download-links {
    margin: 0 auto;
  }
}

@media (max-width: 480px) {
  .footer {
    padding: 30px 10px 15px;
  }

  .footer-section h3 {
    font-size: 14px;
  }

  .footer-section a {
    font-size: 13px;
  }

  .qr-placeholder {
    width: 100px;
    height: 100px;
  }

  .footer-info p {
    font-size: 11px;
  }
}
</style>

