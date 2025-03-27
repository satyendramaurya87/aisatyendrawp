# AI News Scraper & Auto Blogger Pro - होस्टिंग गाइड

## विवरण

यह दस्तावेज़ आपको AI News Scraper & Auto Blogger Pro प्लगइन के लिए API सर्वर को विभिन्न होस्टिंग प्लेटफॉर्म पर सेट करने के बारे में विस्तृत निर्देश प्रदान करता है।

## विकल्प 1: पायथन API को अपने वेब सर्वर पर होस्ट करना

यदि आपके पास अपना वेब सर्वर है और आप उस पर API सेट अप करना चाहते हैं, तो इन चरणों का पालन करें:

### आवश्यकताएँ
- Python 3.10 या उच्चतर
- pip (Python पैकेज इंस्टॉलर)
- SSH एक्सेस (वैकल्पिक, लेकिन अनुशंसित)

### सेटअप चरण

1. **Python इंस्टॉल करें** (यदि पहले से नहीं है):
   ```bash
   # Ubuntu/Debian पर
   sudo apt update
   sudo apt install python3.10 python3.10-venv python3-pip

   # CentOS/RHEL पर
   sudo yum install python3.10 python3.10-devel python3-pip
   ```

2. **प्लगइन फाइलें अपलोड करें**:
   - WordPress प्लगइन डायरेक्टरी (सामान्यतः `wp-content/plugins/`) में `ai-news-scraper-auto-blogger-pro` फोल्डर अपलोड करें
   - अपने सर्वर पर एक अलग स्थान पर `ai-news-scraper-auto-blogger-pro/api` फोल्डर कॉपी करें (उदाहरण के लिए: `/var/www/api-server/`)

3. **वर्चुअल एनवायरनमेंट सेटअप**:
   ```bash
   cd /var/www/api-server/
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

4. **API सर्वर को बैकग्राउंड में चलाने के लिए systemd सर्विस बनाएँ**:
   
   नई फाइल बनाएँ: `/etc/systemd/system/ai-news-api.service`
   ```
   [Unit]
   Description=AI News Scraper API Server
   After=network.target

   [Service]
   User=www-data
   Group=www-data
   WorkingDirectory=/var/www/api-server
   ExecStart=/var/www/api-server/venv/bin/gunicorn --bind 0.0.0.0:5000 --workers 4 main:app
   Restart=always
   Environment="OPENAI_API_KEY=YOUR_OPENAI_API_KEY"

   [Install]
   WantedBy=multi-user.target
   ```

5. **सर्विस शुरू करें**:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start ai-news-api
   sudo systemctl enable ai-news-api
   ```

6. **फायरवॉल कॉन्फ़िगर करें** (वैकल्पिक लेकिन अनुशंसित):
   ```bash
   sudo ufw allow from YOUR_WORDPRESS_SERVER_IP to any port 5000
   ```

7. **WordPress प्लगिन सेटिंग्स में API URL अपडेट करें**:
   - WordPress एडमिन पैनल में जाएँ
   - "AI News Scraper" में जाकर "सेटिंग्स" टैब पर जाएँ
   - API सर्वर URL फील्ड में `http://YOUR_SERVER_IP:5000` या `http://YOUR_DOMAIN:5000` दर्ज करें

## विकल्प 2: Heroku पर होस्टिंग (मुफ्त टियर उपलब्ध)

Heroku एक क्लाउड प्लेटफॉर्म है जो आपके API को होस्ट कर सकता है। यहाँ आप अपने API को Heroku पर होस्ट कर सकते हैं:

### आवश्यकताएँ
- [Heroku](https://www.heroku.com/) खाता
- Git
- Heroku CLI

### सेटअप चरण

1. **Heroku खाता बनाएँ और Heroku CLI इंस्टॉल करें**:
   - [Heroku](https://www.heroku.com/) पर जाकर एक नया खाता बनाएँ
   - [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) इंस्टॉल करें

2. **API डायरेक्टरी को Git रिपॉजिटरी में बदलें**:
   ```bash
   cd /path/to/ai-news-scraper-auto-blogger-pro/api
   git init
   git add .
   git commit -m "Initial commit"
   ```

3. **Heroku ऐप बनाएँ और डिप्लॉय करें**:
   ```bash
   heroku login
   heroku create your-app-name
   git push heroku main
   ```

4. **Heroku पर आवश्यक एनवायरनमेंट वेरिएबल सेट करें**:
   ```bash
   heroku config:set OPENAI_API_KEY=your_openai_api_key
   ```

5. **WordPress प्लगिन सेटिंग्स में API URL अपडेट करें**:
   - WordPress एडमिन पैनल में जाएँ
   - "AI News Scraper" में जाकर "सेटिंग्स" टैब पर जाएँ
   - API सर्वर URL फील्ड में `https://your-app-name.herokuapp.com` दर्ज करें

## विकल्प 3: Vercel पर होस्टिंग (मुफ्त टियर उपलब्ध)

Vercel एक होस्टिंग प्लेटफॉर्म है जो मुफ्त टियर के साथ आता है और Flask ऐप को होस्ट कर सकता है।

### आवश्यकताएँ
- [Vercel](https://vercel.com/) खाता
- GitHub खाता

### सेटअप चरण

1. **Vercel खाता बनाएँ**:
   - [Vercel](https://vercel.com/) पर जाकर GitHub, GitLab, या Bitbucket अकाउंट से साइन अप करें

2. **GitHub पर अपनी API को पुश करें**:
   - GitHub पर एक नया रिपॉजिटरी बनाएँ
   - अपने API फोल्डर को इस रिपॉजिटरी में पुश करें:
     ```bash
     cd /path/to/ai-news-scraper-auto-blogger-pro/api
     git init
     git add .
     git commit -m "Initial commit"
     git remote add origin https://github.com/your-username/your-repo.git
     git push -u origin main
     ```

3. **Vercel पर नया प्रोजेक्ट बनाएँ**:
   - Vercel डैशबोर्ड में जाएँ
   - "New Project" पर क्लिक करें
   - अपने GitHub रिपॉजिटरी को आयात करें
   - बिल्ड सेटिंग्स कॉन्फ़िगर करें:
     - Framework Preset: Other
     - Build Command: pip install -r requirements.txt
     - Output Directory: .
     - Environment Variables: OPENAI_API_KEY=your_openai_api_key
   - "Deploy" पर क्लिक करें

4. **WordPress प्लगिन सेटिंग्स में API URL अपडेट करें**:
   - WordPress एडमिन पैनल में जाएँ
   - "AI News Scraper" में जाकर "सेटिंग्स" टैब पर जाएँ
   - API सर्वर URL फील्ड में Vercel ऐप URL (जैसे `https://your-app.vercel.app`) दर्ज करें

## विकल्प 4: Railway पर होस्टिंग (मुफ्त टियर उपलब्ध)

Railway एक और आधुनिक क्लाउड प्लेटफॉर्म है जो Python ऐप्स की होस्टिंग के लिए उपयुक्त है।

### आवश्यकताएँ
- [Railway](https://railway.app/) खाता
- GitHub खाता

### सेटअप चरण

1. **Railway खाता बनाएँ**:
   - [Railway](https://railway.app/) पर जाकर GitHub से साइन अप करें

2. **GitHub पर अपनी API को पुश करें** (विकल्प 3 की तरह)

3. **Railway पर नया प्रोजेक्ट बनाएँ**:
   - Railway डैशबोर्ड में जाएँ
   - "New Project" पर क्लिक करें
   - "Deploy from GitHub repo" चुनें
   - अपने रिपॉजिटरी को चुनें
   - Variables टैब में OPENAI_API_KEY जोड़ें
   - स्वचालित डिप्लॉयमेंट शुरू हो जाएगा

4. **WordPress प्लगिन सेटिंग्स में API URL अपडेट करें**:
   - WordPress एडमिन पैनल में जाएँ
   - "AI News Scraper" में जाकर "सेटिंग्स" टैब पर जाएँ
   - API सर्वर URL फील्ड में Railway द्वारा प्रदान किए गए URL दर्ज करें

## सुरक्षा अनुशंसाएँ

1. **हमेशा सुरक्षित कनेक्शन का उपयोग करें**:
   - API के लिए HTTPS कनेक्शन का उपयोग करें
   - अपने WordPress साइट के लिए SSL/TLS सर्टिफिकेट सुनिश्चित करें

2. **API की के सुरक्षा**:
   - OpenAI API की जैसे संवेदनशील जानकारी को एनवायरनमेंट वेरिएबल के रूप में स्टोर करें
   - API की को कोड में हार्डकोड न करें

3. **एक्सेस नियंत्रण**:
   - यदि संभव हो तो, API एक्सेस को अपने WordPress सर्वर के IP से ही प्रतिबंधित करें
   - API प्रतिक्रियाओं में संवेदनशील जानकारी शामिल न करें

## समस्या निवारण

1. **API कनेक्शन समस्याएँ**:
   - सुनिश्चित करें कि API URL सही है और कोई टाइपिंग त्रुटि नहीं है
   - फायरवॉल या सिक्योरिटी ग्रुप्स में पोर्ट 5000 (या आपका कॉन्फ़िगर किया गया पोर्ट) खुला है
   - API लॉग्स की जांच करें

2. **OpenAI API समस्याएँ**:
   - सुनिश्चित करें कि आपकी OpenAI API की मान्य है और उसमें पर्याप्त क्रेडिट हैं
   - API की एरर मैसेज को लॉग फाइल में देखें
   - OpenAI डैशबोर्ड पर API उपयोग की जांच करें

3. **परफॉरमेंस समस्याएँ**:
   - बड़े लेखों के लिए, API अनुरोध को पूरा होने में समय लग सकता है
   - यदि API टाइमआउट हो रही है, तो gunicorn workers की संख्या बढ़ाएँ
   - बड़े पैमाने पर उपयोग के लिए, आप अधिक शक्तिशाली सर्वर प्लान पर अपग्रेड करने पर विचार कर सकते हैं