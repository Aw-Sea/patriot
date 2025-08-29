---
title: Sidebar
routable: false
visible: false
process:
    twig: true
    markdown: false
---

   
  <style>

.compact-calendar {
  max-width: 300px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
  font-size: 14px;
}

.memorial-info {
  background: #fff8e1;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
  text-align: center;
}

.memorial-info h3 {
  margin: 0 0 5px 0;
  font-size: 14px;
  color: #5d4037;
}

#next-memorial-date {
  font-weight: bold;
  color: #d32f2f;
}

.compact-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 3px;
}

.compact-day {
  padding: 5px;
  text-align: center;
  border-radius: 3px;
  font-size: 12px;
  position: relative;
  min-height: 25px;
}

.day-header {
  font-weight: bold;
  background: #f5f5f5;
  padding: 5px;
}

.today {
  background: #4caf50;
  color: white;
}

.memorial-day {
  background: #ffcdd2;
  color: #c62828;
  font-weight: bold;
}

.next-memorial {
  box-shadow: 0 0 0 2px #ff9800;
}

.compact-legend {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 10px;
  font-size: 12px;
}

.today-marker, .memorial-marker {
  display: inline-block;
  width: 12px;
  height: 12px;
  border-radius: 2px;
  margin-right: 3px;
  vertical-align: middle;
}

.today-marker {
  background: #4caf50;
}

.memorial-marker {
  background: #ffcdd2;
}

  </style>


 <!-- Боковая панель -->
        <div class="mb-4 p-3 bg-light border rounded">
    	<b><a href="https://xn--80alddaxhcej6afd4kge.xn--p1ai/ru/proekty" target="_blank"> Наши проекты</a></b>
        </div>

   
        <div class="mb-4 p-3 bg-light border rounded">
    	<b><a href="https://xn--80alddaxhcej6afd4kge.xn--p1ai/ru/partnery" target="_blank"> Наши партнёры</a></b>
        </div>
 



<div class="compact-calendar">
    <div class="memorial-info" id="memorial-info">
      <b>Ближайшая памятная дата:</b>
      <div id="next-memorial-date">9 мая - День Победы (1945)</div>
    </div>

    <div class="calendar-controls" align="center">
      <button id="prev-month">←</button>
      <b id="current-month-year">Май 2024</b>
      <button id="next-month">→</button>
    </div>

    <div class="compact-grid" id="calendar"></div>

    <div class="compact-legend">
      <span class="today-marker"></span> Сегодня
      <span class="memorial-marker"></span> Памятная дата
    </div>
  </div>












  <script>

    // Расширенный список памятных дат
const memorialDates = [
  { month: 0, day: 7, name: 'Рождественский штурм Грозного (1995)' },
  { month: 0, day: 27, name: 'День снятия блокады Ленинграда (1944)' },
  { month: 1, day: 2, name: 'День разгрома фашистов в Сталинграде (1943)' },
  { month: 1, day: 15, name: 'День вывода войск из Афганистана (1989)' },
  { month: 1, day: 23, name: 'День защитника Отечества' },
  { month: 2, day: 18, name: 'Взятие крепости Корфу (1799, Ушаков)' },
  { month: 3, day: 4, name: 'Освобождение Одессы (1944)' },
  { month: 3, day: 18, name: 'Ледовое побоище (1242)' },
  { month: 4, day: 6, name: 'Освобождение Праги (1945)' },
  { month: 4, day: 9, name: 'День Победы (1945)' },
  { month: 4, day: 12, name: 'Крымская наступательная операция (1944)' },
  { month: 4, day: 28, name: 'Брусиловский прорыв (1916)' },
  { month: 5, day: 22, name: 'Начало Великой Отечественной войны (1941)' },
  { month: 5, day: 29, name: 'День партизан и подпольщиков' },
  { month: 6, day: 7, name: 'День победы русского флота (Чесма, 1770)' },
  { month: 6, day: 10, name: 'Победа при Полтаве (1709)' },
  { month: 6, day: 15, name: 'Невская битва (1240)' },
  { month: 6, day: 23, name: 'Куликовская битва (1380)' },
  { month: 7, day: 1, name: 'День памяти жертв Первой мировой войны' },
  { month: 7, day: 9, name: 'Победа у мыса Гангут (1714)' },
  { month: 7, day: 23, name: 'Курская битва (1943)' },
  { month: 8, day: 8, name: 'День Бородинского сражения (1812)' },
  { month: 8, day: 11, name: 'Победа у мыса Тендра (1790)' },
  { month: 8, day: 21, name: 'Победа на Куликовом поле (1380)' },
  { month: 9, day: 1, name: 'День Сухопутных войск РФ' },
  { month: 9, day: 14, name: 'Освобождение Москвы (Народное ополчение, 1612)' },
  { month: 9, day: 18, name: 'Битва при Нарве (1700)' },
  { month: 10, day: 4, name: 'День народного единства' },
  { month: 10, day: 7, name: 'День военного парада на Красной площади (1941)' },
  { month: 10, day: 19, name: 'День ракетных войск и артиллерии' },
  { month: 10, day: 24, name: 'Взятие турецкой крепости Измаил (1790)' },
  { month: 11, day: 1, name: 'День победы русской эскадры (Синоп, 1853)' },
  { month: 11, day: 5, name: 'Начало контрнаступления под Москвой (1941)' },
  { month: 11, day: 17, name: 'Взятие крепости Очаков (1788)' },
  { month: 11, day: 24, name: 'День взятия турецкой крепости Карс (1877)' }
];

document.addEventListener('DOMContentLoaded', function() {
  let currentDate = new Date();
  renderCalendar(currentDate);
  updateNextMemorialInfo();

  document.getElementById('prev-month').addEventListener('click', () => {
    currentDate.setMonth(currentDate.getMonth() - 1);
    renderCalendar(currentDate);
  });

  document.getElementById('next-month').addEventListener('click', () => {
    currentDate.setMonth(currentDate.getMonth() + 1);
    renderCalendar(currentDate);
  });
});

function renderCalendar(date) {
  const calendarEl = document.getElementById('calendar');
  calendarEl.innerHTML = '';

  // Заголовки дней недели
  const daysOfWeek = ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'];
  daysOfWeek.forEach(day => {
    const dayEl = document.createElement('div');
    dayEl.className = 'compact-day day-header';
    dayEl.textContent = day;
    calendarEl.appendChild(dayEl);
  });

  // Установка заголовка месяца
  const monthNames = ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь',
                     'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'];
  document.getElementById('current-month-year').textContent =
    `${monthNames[date.getMonth()]} ${date.getFullYear()}`;

  // Первый день месяца
  const firstDay = new Date(date.getFullYear(), date.getMonth(), 1);
  // Последний день месяца
  const lastDay = new Date(date.getFullYear(), date.getMonth() + 1, 0);

  // Пустые ячейки перед первым днем
  for (let i = 0; i < (firstDay.getDay() || 7) - 1; i++) {
    calendarEl.appendChild(document.createElement('div'));
  }

  // Дни месяца
  const today = new Date();
  const nextMemorial = getNextMemorialDate();

  for (let day = 1; day <= lastDay.getDate(); day++) {
    const dayEl = document.createElement('div');
    dayEl.className = 'compact-day';
    dayEl.textContent = day;

    // Проверка на сегодняшний день
    if (date.getMonth() === today.getMonth() &&
        date.getFullYear() === today.getFullYear() &&
        day === today.getDate()) {
      dayEl.classList.add('today');
    }

    // Проверка на памятные даты
    const isMemorialDay = memorialDates.some(md =>
      md.month === date.getMonth() && md.day === day);

    if (isMemorialDay) {
      dayEl.classList.add('memorial-day');
      const memorialInfo = memorialDates.find(md =>
        md.month === date.getMonth() && md.day === day);
      dayEl.title = memorialInfo.name;

      // Если это ближайшая памятная дата
      if (nextMemorial && day === nextMemorial.date.getDate() &&
          date.getMonth() === nextMemorial.date.getMonth()) {
        dayEl.classList.add('next-memorial');
      }
    }

    calendarEl.appendChild(dayEl);
  }
}

// Функция для обновления блока с ближайшей датой
function updateNextMemorialInfo() {
  const nextMemorial = getNextMemorialDate();
  if (nextMemorial) {
    const monthNames = ['января', 'февраля', 'марта', 'апреля', 'мая', 'июня',
                       'июля', 'августа', 'сентября', 'октября', 'ноября', 'декабря'];
    const formattedDate = `${nextMemorial.date.getDate()} ${monthNames[nextMemorial.date.getMonth()]}`;
    document.getElementById('next-memorial-date').textContent =
      `${formattedDate} - ${nextMemorial.name}`;
  }
}

// Функция для поиска ближайшей памятной даты
function getNextMemorialDate() {
  const today = new Date();
  const currentYear = today.getFullYear();

  // Создаем массив всех памятных дат в этом году
  const datesThisYear = memorialDates.map(md => ({
    date: new Date(currentYear, md.month, md.day),
    name: md.name
  }));

  // Добавляем даты на следующий год
  const datesNextYear = memorialDates.map(md => ({
    date: new Date(currentYear + 1, md.month, md.day),
    name: md.name
  }));

  const allDates = [...datesThisYear, ...datesNextYear];

  // Находим ближайшую дату
  const nextDate = allDates
    .filter(d => d.date >= new Date(today.setHours(0, 0, 0, 0)))
    .sort((a, b) => a.date - b.date)[0];

  return nextDate;
}
  </script>