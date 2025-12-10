// ============================================
// БУЧА ЦИФРОВА - ГОЛОВНИЙ СКРИПТ
// ============================================

let servicesData = [...SERVICES_CONFIG];
let activeService = null;

// ============================================
// ІНІЦІАЛІЗАЦІЯ
// ============================================

document.addEventListener('DOMContentLoaded', () => {
    renderServices();
    setupEventListeners();
    updateStats();
});

// ============================================
// РЕНДЕРИНГ СЕРВІСІВ
// ============================================

function renderServices(searchQuery = '') {
    const grid = document.getElementById('servicesGrid');
    grid.innerHTML = '';

    // Фільтрація сервісів
    const filteredServices = servicesData.filter(service =>
        service.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
        service.description.toLowerCase().includes(searchQuery.toLowerCase())
    );

    // Рендер карток сервісів
    filteredServices.forEach(service => {
        const card = createServiceCard(service);
        grid.appendChild(card);
    });

    // Додати кнопку "Додати сервіс"
    const addCard = createAddServiceCard();
    grid.appendChild(addCard);
}

function createServiceCard(service) {
    const colors = COLOR_SCHEMES[service.color];
    
    const card = document.createElement('div');
    card.className = 'service-card';
    card.style.setProperty('--card-color', colors.primary);
    card.style.setProperty('--card-light', colors.light);
    card.style.setProperty('--card-gradient', colors.gradient);
    
    card.innerHTML = `
        <div class="service-icon">
            <i class="fas ${service.icon}"></i>
        </div>
        <h3 class="service-title">${service.title}</h3>
        <p class="service-description">${service.description}</p>
        <div class="service-footer">
            <span>Відкрити</span>
            <i class="fas fa-chevron-right"></i>
        </div>
    `;

    card.addEventListener('click', () => openServiceModal(service));
    
    return card;
}

function createAddServiceCard() {
    const card = document.createElement('div');
    card.className = 'service-card add-service-card';
    
    card.innerHTML = `
        <div class="add-service-icon">
            <i class="fas fa-plus"></i>
        </div>
        <h3 class="service-title">Додати сервіс</h3>
        <p class="service-description">Розширте функціонал додатку</p>
    `;

    card.addEventListener('click', openAddServiceModal);
    
    return card;
}

// ============================================
// МОДАЛЬНІ ВІКНА
// ============================================

function openServiceModal(service) {
    activeService = service;
    const modal = document.getElementById('serviceModal');
    const colors = COLOR_SCHEMES[service.color];
    
    // Встановлення заголовку
    const header = document.getElementById('modalHeader');
    header.style.background = colors.gradient;
    
    document.getElementById('modalIcon').innerHTML = `<i class="fas ${service.icon}"></i>`;
    document.getElementById('modalTitle').textContent = service.title;
    document.getElementById('modalDescription').textContent = service.description;
    
    // Рендер контенту
    const body = document.getElementById('modalBody');
    body.innerHTML = renderServiceContent(service, colors);
    
    modal.classList.add('active');
    document.body.style.overflow = 'hidden';
}

function closeServiceModal() {
    const modal = document.getElementById('serviceModal');
    modal.classList.remove('active');
    document.body.style.overflow = '';
    activeService = null;
}

function renderServiceContent(service, colors) {
    switch(service.id) {
        case 'announcements':
            return renderAnnouncements(service.content, colors);
        case 'services':
            return renderServices_Content(service.content, colors);
        case 'contacts':
            return renderContacts(service.content, colors);
        case 'map':
            return renderMap(service.content, colors);
        case 'events':
            return renderEvents(service.content, colors);
        default:
            return renderCustomContent(service.content, colors);
    }
}

function renderAnnouncements(content, colors) {
    return content.map(item => `
        <div class="announcement-item" style="background: ${colors.light}; border-color: ${colors.primary}">
            <div class="announcement-header">
                <h3>${item.title}</h3>
                <span class="announcement-date" style="color: ${colors.primary}">${item.date}</span>
            </div>
            <p>${item.text}</p>
        </div>
    `).join('');
}

function renderServices_Content(content, colors) {
    return content.map(item => `
        <div class="service-item">
            <div class="service-item-content">
                <h3>${item.name}</h3>
                <div class="service-item-details">
                    <span><i class="far fa-clock"></i> ${item.time}</span>
                    <span><i class="fas fa-map-marker-alt"></i> ${item.location}</span>
                </div>
                ${item.description ? `<p class="service-item-description">${item.description}</p>` : ''}
            </div>
            <span class="service-status status-${item.status === 'Онлайн' ? 'online' : 'booking'}">${item.status}</span>
        </div>
    `).join('');
}

function renderContacts(content, colors) {
    const grouped = {};
    content.forEach(item => {
        const category = item.address === 'Екстрена служба' ? 'Екстрені служби' : 
                        item.address.includes('Аварійна') ? 'Аварійні служби' : 'Установи';
        if (!grouped[category]) grouped[category] = [];
        grouped[category].push(item);
    });

    return Object.keys(grouped).map(category => `
        <div class="contacts-category">
            <h3 class="category-title">${category}</h3>
            <div class="contacts-grid">
                ${grouped[category].map(item => `
                    <div class="contact-card" style="background: ${colors.light}; border-color: ${colors.primary}">
                        <h4>${item.name}</h4>
                        <a href="tel:${item.phone}" class="contact-phone" style="color: ${colors.primary}">${item.phone}</a>
                        <p class="contact-hours"><i class="far fa-clock"></i> ${item.hours}</p>
                        ${item.address && item.address !== 'Екстрена служба' && item.address !== 'Аварійна служба' ? 
                            `<p class="contact-address"><i class="fas fa-map-marker-alt"></i> ${item.address}</p>` : ''}
                    </div>
                `).join('')}
            </div>
        </div>
    `).join('');
}

function renderMap(content, colors) {
    const categories = [...new Set(content.map(item => item.category))];
    
    return `
        <div class="map-placeholder">
            <i class="fas fa-map-marked-alt"></i>
            <p>Інтерактивна карта</p>
            <small>На реальному сайті тут буде Google Maps / OpenStreetMap</small>
        </div>
        ${categories.map(category => `
            <div class="map-category">
                <h3 class="category-title">${category}</h3>
                <div class="map-locations">
                    ${content.filter(item => item.category === category).map(item => `
                        <div class="location-card">
                            <h4>${item.name}</h4>
                            <p class="location-address"><i class="fas fa-map-marker-alt"></i> ${item.address}</p>
                            ${item.phone ? `<p class="location-phone"><i class="fas fa-phone"></i> ${item.phone}</p>` : ''}
                            ${item.hours ? `<p class="location-hours"><i class="far fa-clock"></i> ${item.hours}</p>` : ''}
                        </div>
                    `).join('')}
                </div>
            </div>
        `).join('')}
    `;
}

function renderEvents(content, colors) {
    return content.map(item => {
        const [day, month, year] = item.date.split('.');
        return `
            <div class="event-item" style="background: ${colors.light}; border-color: ${colors.primary}">
                <div class="event-date" style="color: ${colors.primary}">
                    <div class="event-day">${day}</div>
                    <div class="event-month">${month}.${year}</div>
                </div>
                <div class="event-content">
                    <h3>${item.title}</h3>
                    <div class="event-details">
                        <span><i class="far fa-clock"></i> ${item.time}</span>
                        <span><i class="fas fa-map-marker-alt"></i> ${item.location}</span>
                        <span class="event-category" style="background: ${colors.primary}">${item.category}</span>
                    </div>
                    ${item.description ? `<p class="event-description">${item.description}</p>` : ''}
                </div>
            </div>
        `;
    }).join('');
}

function renderCustomContent(content, colors) {
    if (Array.isArray(content)) {
        return content.map(item => `
            <div class="custom-item" style="background: ${colors.light}; border-color: ${colors.primary}">
                <pre>${JSON.stringify(item, null, 2)}</pre>
            </div>
        `).join('');
    }
    return `<div class="custom-content">${JSON.stringify(content)}</div>`;
}

// ============================================
// ДОДАВАННЯ НОВОГО СЕРВІСУ
// ============================================

function openAddServiceModal() {
    const modal = document.getElementById('addServiceModal');
    modal.classList.add('active');
    document.body.style.overflow = 'hidden';
}

function closeAddServiceModal() {
    const modal = document.getElementById('addServiceModal');
    modal.classList.remove('active');
    document.body.style.overflow = '';
    document.getElementById('addServiceForm').reset();
}

function addNewService(formData) {
    const newService = {
        id: 'custom_' + Date.now(),
        title: formData.get('serviceName'),
        icon: formData.get('serviceIcon'),
        color: formData.get('serviceColor'),
        description: formData.get('serviceDescription'),
        content: []
    };

    servicesData.push(newService);
    
    // Зберегти в localStorage
    localStorage.setItem('customServices', JSON.stringify(servicesData));
    
    renderServices();
    updateStats();
    closeAddServiceModal();
    
    alert('Сервіс успішно додано!');
}

// ============================================
// ПОШУК
// ============================================

function setupSearch() {
    const searchInput = document.getElementById('searchInput');
    searchInput.addEventListener('input', (e) => {
        renderServices(e.target.value);
    });
}

// ============================================
// СТАТИСТИКА
// ============================================

function updateStats() {
    document.getElementById('statServices').textContent = servicesData.length;
}

// ============================================
// EVENT LISTENERS
// ============================================

function setupEventListeners() {
    // Пошук
    setupSearch();
    
    // Закриття модального вікна сервісу
    document.getElementById('modalClose').addEventListener('click', closeServiceModal);
    
    // Закриття модального вікна додавання
    document.getElementById('addModalClose').addEventListener('click', closeAddServiceModal);
    document.getElementById('cancelAddService').addEventListener('click', closeAddServiceModal);
    
    // Закриття при кліку поза модальним вікном
    document.getElementById('serviceModal').addEventListener('click', (e) => {
        if (e.target.id === 'serviceModal') {
            closeServiceModal();
        }
    });
    
    document.getElementById('addServiceModal').addEventListener('click', (e) => {
        if (e.target.id === 'addServiceModal') {
            closeAddServiceModal();
        }
    });
    
    // Форма додавання сервісу
    document.getElementById('addServiceForm').addEventListener('submit', (e) => {
        e.preventDefault();
        const formData = new FormData(e.target);
        addNewService(formData);
    });
    
    // Кнопка користувача (заглушка)
    document.getElementById('userBtn').addEventListener('click', () => {
        alert('Функція авторизації буде доступна в наступній версії');
    });
    
    // Escape key для закриття модальних вікон
    document.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') {
            closeServiceModal();
            closeAddServiceModal();
        }
    });
}

// ============================================
// ЗАВАНТАЖЕННЯ ЗБЕРЕЖЕНИХ СЕРВІСІВ
// ============================================

function loadCustomServices() {
    const saved = localStorage.getItem('customServices');
    if (saved) {
        try {
            servicesData = JSON.parse(saved);
        } catch (e) {
            console.error('Помилка завантаження збережених сервісів:', e);
        }
    }
}

// Завантажити збережені сервіси при старті
loadCustomServices();
