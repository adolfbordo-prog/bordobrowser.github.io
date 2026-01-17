// العناصر الرئيسية
const webview = document.getElementById('عرض-الويب');
const urlBar = document.getElementById('شريط-العنوان');
const goButton = document.getElementById('اذهب');
const backButton = document.getElementById('للخلف');
const forwardButton = document.getElementById('للأمام');
const refreshButton = document.getElementById('تحديث');
const homeButton = document.getElementById('الصفحة-الرئيسية');
const bookmarksButton = document.getElementById('الإشارات-المرجعية');
const bookmarksList = document.getElementById('قائمة-الإشارات');
const statusText = document.getElementById('حالة-التحميل');
const currentPageTitle = document.getElementById('عنوان-الصفحة-الحالية');

// الصفحة الرئيسية
const HOME_PAGE = 'https://www.google.com';

// تهيئة المتصفح
function تهيئةالمتصفح() {
    // تحميل الصفحة الرئيسية
    تحميلالصفحة(HOME_PAGE);
    
    // إضافة الإشارات المرجعية
    إضافةالإشارات();
    
    // إضافة مستمعي الأحداث
    إضافةالمستمعين();
}

// تحميل صفحة
function تحميلالصفحة(url) {
    if (!url.startsWith('http')) {
        url = 'https://' + url;
    }
    
    statusText.textContent = 'جاري التحميل...';
    statusText.style.color = '#4a6491';
    
    webview.src = url;
    urlBar.value = url;
}

// إضافة المستمعين للأحداث
function إضافةالمستمعين() {
    // زر الذهاب
    goButton.addEventListener('click', () => {
        const url = urlBar.value.trim();
        if (url) {
            تحميلالصفحة(url);
        }
    });
    
    // Enter في شريط العنوان
    urlBar.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            const url = urlBar.value.trim();
            if (url) {
                تحميلالصفحة(url);
            }
        }
    });
    
    // أزرار التنقل
    backButton.addEventListener('click', () => {
        try {
            webview.contentWindow.history.back();
        } catch (error) {
            console.log('لا يمكن الرجوع للخلف');
        }
    });
    
    forwardButton.addEventListener('click', () => {
        try {
            webview.contentWindow.history.forward();
        } catch (error) {
            console.log('لا يمكن التقدم للأمام');
        }
    });
    
    refreshButton.addEventListener('click', () => {
        webview.contentWindow.location.reload();
    });
    
    homeButton.addEventListener('click', () => {
        تحميلالصفحة(HOME_PAGE);
    });
    
    // الإشارات المرجعية
    bookmarksButton.addEventListener('click', () => {
        bookmarksList.classList.toggle('مخفي');
    });
    
    // تحديث حالة التحميل
    webview.addEventListener('load', () => {
        statusText.textContent = 'تم التحميل';
        statusText.style.color = '#28a745';
        
        try {
            const title = webview.contentDocument.title;
            currentPageTitle.textContent = title || webview.src;
        } catch (error) {
            currentPageTitle.textContent = webview.src;
        }
    });
    
    webview.addEventListener('error', () => {
        statusText.textContent = 'خطأ في التحميل';
        statusText.style.color = '#dc3545';
    });
    
    // أزرار التحكم بالنافذة
    document.getElementById('تصغير').addEventListener('click', () => {
        alert('زر التصغير - تحت التطوير');
    });
    
    document.getElementById('تكبير').addEventListener('click', () => {
        alert('زر التكبير - تحت التطوير');
    });
    
    document.getElementById('إغلاق').addEventListener('click', () => {
        if (confirm('هل تريد إغلاق المتصفح؟')) {
            document.body.innerHTML = '<h1 style="text-align:center;padding:50px;">تم إغلاق المتصفح</h1>';
        }
    });
}

// إضافة الإشارات المرجعية
function إضافةالإشارات() {
    const bookmarks = document.querySelectorAll('#قائمة-المواقع a');
    
    bookmarks.forEach(bookmark => {
        bookmark.addEventListener('click', (e) => {
            e.preventDefault();
            const url = bookmark.getAttribute('data-url');
            تحميلالصفحة(url);
            bookmarksList.classList.add('مخفي');
        });
    });
    
    // زر إضافة إشارة جديدة
    document.getElementById('إضافة-إشارة').addEventListener('click', () => {
        const url = prompt('أدخل عنوان الموقع:', 'https://');
        const name = prompt('أدخل اسم الموقع:', 'موقع جديد');
        
        if (url && name) {
            const newBookmark = document.createElement('li');
            newBookmark.innerHTML = `<a href="#" data-url="${url}">${name}</a>`;
            document.getElementById('قائمة-المواقع').appendChild(newBookmark);
            إضافةالإشارات(); // إعادة تهيئة المستمعين
        }
    });
}

// تهيئة المتصفح عند تحميل الصفحة
window.addEventListener('DOMContentLoaded', تهيئةالمتصفح);

// دعم الأوامر من وحدة التحكم
console.log('مرحباً بك في المتصفح العربي!');
console.log('الأوامر المتاحة:');
console.log('تحميلالصفحة("https://example.com") - لتحميل صفحة جديدة');
