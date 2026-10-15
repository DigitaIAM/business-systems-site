<script setup>
import { ref, watch } from 'vue'

// Управление активным проектом в модалке / секции кейсов
const activeProject = ref(null)

const openProject = (project) => {
  activeProject.value = project
}

const closeProject = () => {
  activeProject.value = null
}

// Увеличение скриншота по клику
const zoomedImage = ref(null)

const toggleZoom = (src) => {
  zoomedImage.value = zoomedImage.value === src ? null : src
}

// Блокировка прокрутки фона при открытой модалке
watch(activeProject, (project) => {
  document.body.style.overflow = project ? 'hidden' : ''
})

// Форма заявки
const form = ref({ name: '', phone: '', company: '', message: '' })
const isSubmitted = ref(false)
const isSubmitting = ref(false)
const submitError = ref(false)

const FORMSPREE_ENDPOINT = 'https://formspree.io/f/xgaonbqz'

const submitForm = async () => {
  if (!form.value.name || !form.value.phone) return
  isSubmitting.value = true
  submitError.value = false

  const data = new FormData()
  data.append('name', form.value.name)
  data.append('phone', form.value.phone)
  data.append('company', form.value.company)
  data.append('message', form.value.message)

  try {
    const res = await fetch(FORMSPREE_ENDPOINT, {
      method: 'POST',
      body: data,
      headers: { Accept: 'application/json' },
    })
    if (res.ok) {
      isSubmitted.value = true
      form.value = { name: '', phone: '', company: '', message: '' }
    } else {
      submitError.value = true
    }
  } catch {
    submitError.value = true
  } finally {
    isSubmitting.value = false
  }
}
</script>

<template>
  <div class="min-h-screen bg-slate-50 text-slate-800 font-sans selection:bg-blue-500 selection:text-white">

    <!-- Навигация -->
    <header class="sticky top-0 z-40 bg-white/80 backdrop-blur-md border-b border-slate-200">
      <div class="max-w-7xl mx-auto px-6 h-20 flex items-center justify-between">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-blue-600 flex items-center justify-center text-white font-bold text-xl shadow-lg shadow-blue-500/30">
            DI
          </div>
          <span class="text-xl font-extrabold tracking-tight text-slate-900">DigitaIAM<span class="text-blue-600"></span></span>
        </div>
        <nav class="hidden md:flex items-center space-x-8 text-sm font-medium text-slate-600">
          <a href="#products" class="hover:text-blue-600 transition">Продукты</a>
          <a href="#features" class="hover:text-blue-600 transition">Преимущества</a>
          <a href="#problems" class="hover:text-blue-600 transition">Реальные проблемы и решения</a>
          <a href="#contact" class="px-5 py-2.5 rounded-xl bg-blue-600 text-white font-semibold hover:bg-blue-700 transition shadow-md shadow-blue-500/20">Обсудить проект</a>
        </nav>
      </div>
    </header>

    <!-- Hero Сеция (Вариант 1: Светлый технологичный фон) -->
    <section class="relative min-h-[calc(100vh-80px)] flex items-center pt-16 pb-20 overflow-hidden bg-gradient-to-b from-blue-50/50 to-slate-50">
      <!-- Декоративный светлый фон с сеткой и градиентными пятнами -->
      <div class="absolute inset-0 bg-[linear-gradient(to_right,#e2e8f0_1px,transparent_1px),linear-gradient(to_bottom,#e2e8f0_1px,transparent_1px)] bg-[size:4rem_4rem]"></div>
      <div class="absolute -top-40 -left-40 w-96 h-96 bg-blue-200/40 rounded-full blur-[128px] pointer-events-none"></div>
      <div class="absolute -bottom-40 -right-40 w-96 h-96 bg-indigo-200/40 rounded-full blur-[128px] pointer-events-none"></div>

      <div class="relative max-w-5xl mx-auto px-6 text-center space-y-8">

        <h1 class="text-4xl sm:text-6xl lg:text-7xl font-extrabold tracking-tight leading-tight text-slate-900">
          Единое цифровое ядро <br class="hidden sm:inline" />
          <span class="text-transparent bg-clip-text bg-gradient-to-r from-blue-600 via-indigo-600 to-emerald-500">для бизнеса</span>
        </h1>

        <p class="max-w-3xl mx-auto text-lg sm:text-xl text-slate-600 leading-relaxed font-normal">
          Тотальный перекрестный контроль персонала и всех процессов от закупа до касс. Предотвращает ошибки персонала. Позволяет работать с неквалифицированным персоналом без долгих месяцев обучения. Полная прозрачность доходов, пересчет остатков в реальном времени и защита от потерь.
        </p>

        <div class="flex flex-col sm:flex-row items-center justify-center gap-4 pt-4">
          <a href="#products" class="w-full sm:w-auto px-8 py-4 rounded-xl bg-blue-600 hover:bg-blue-700 text-white font-semibold shadow-lg shadow-blue-500/25 transition">
            Изучить решения
          </a>
        </div>
      </div>
    </section>

    <!-- Продукты компании (Флагманские кейсы) -->
    <section id="products" class="min-h-screen flex flex-col justify-center py-24 max-w-7xl mx-auto px-6">

      <div class="grid lg:grid-cols-2 gap-10">

        <!-- Карточка 1: Dori.pro -->
        <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between">
          <div class="space-y-6">
            <div class="flex items-center justify-between">
              <span class="px-3.5 py-1 bg-emerald-50 text-emerald-600 text-xs font-bold rounded-full">Фармацевтика & Ритейл</span>
              <span class="text-slate-400 text-sm">Мультиплатформа</span>
            </div>
            <h3 class="text-2xl font-bold text-slate-900">Dori.pro — Управление аптечной сетью</h3>

            <!-- Обновленное описание -->
            <p class="text-slate-600 text-sm leading-relaxed">
              В отличие от тяжелых 1С, SAP или стандартных WMS, Dori.pro — это адаптивная система, которая в реальном времени автоматически планирует закупки и распределяет товар от склада до касс, предотвращая ошибки персонала. Не требует долгих месяцев обучения.
            </p>

            <ul class="space-y-2.5 text-sm text-slate-700">
              <li class="flex items-center"><span class="text-emerald-500 mr-2">✓</span> Автораспределение товара с учетом спроса и сроков (FEFO)</li>
              <li class="flex items-center"><span class="text-emerald-500 mr-2">✓</span> Защита от ошибок неквалифицированного персонала</li>
              <li class="flex items-center"><span class="text-emerald-500 mr-2">✓</span> Пересчет остатков в реальном времени и выявление аномалий</li>
            </ul>
          </div>
          <div class="pt-8">
            <button @click="openProject('dori')" class="w-full py-3 rounded-xl bg-slate-900 text-white font-semibold hover:bg-emerald-600 transition">
              Подробнее о системе
            </button>
          </div>
        </div>

        <!-- Карточка 2: Trucks.land -->
        <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between">
          <div class="space-y-6">
            <div class="flex items-center justify-between">
              <span class="px-3.5 py-1 bg-blue-50 text-blue-600 text-xs font-bold rounded-full">Логистика & TMS</span>
              <span class="text-slate-400 text-sm">Облачное решение</span>
            </div>
            <h3 class="text-2xl font-bold text-slate-900">Trucks.land — Управление грузоперевозками</h3>
            <p class="text-slate-600 text-sm leading-relaxed">
              Облачная система для транспортных компаний: заказы, живой трекинг в реальном времени, автоматические расчеты с владельцами тягачей и диспетчерами, факторинг и строгая отчетность.
            </p>
            <ul class="space-y-2.5 text-sm text-slate-700">
              <li class="flex items-center"><span class="text-blue-500 mr-2">✓</span> Единое рабочее место для диспетчеров и руководства</li>
              <li class="flex items-center"><span class="text-blue-500 mr-2">✓</span> Живой дашборд статусов заказов и маршрутов</li>
              <li class="flex items-center"><span class="text-blue-500 mr-2">✓</span> Автоматические расчеты зарплат, комиссий и выплат</li>
            </ul>
          </div>
          <div class="pt-8">
            <button @click="openProject('trucks')" class="w-full py-3 rounded-xl bg-slate-900 text-white font-semibold hover:bg-blue-600 transition">
              Подробнее о системе
            </button>
          </div>
        </div>

      </div>
    </section>


    <!-- Секция Преимущества -->
    <section id="features" class="min-h-screen flex items-center py-24 bg-white border-y border-slate-100">
      <div class="max-w-7xl mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16 space-y-4">
          <h2 class="text-3xl lg:text-4xl font-extrabold text-slate-900 mt-6">Почему бизнес выбирает нас</h2>
          <p class="text-slate-600">Мы заменяем ручной труд, таблицы Excel и хаос в процессах на умные облачные алгоритмы</p>
        </div>

        <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">

          <!-- Преимущество 1 -->
          <div class="p-8 rounded-3xl bg-slate-50 border border-slate-200/80 hover:border-blue-300 hover:shadow-xl transition space-y-4">
            <div class="w-12 h-12 rounded-2xl bg-blue-600 text-white flex items-center justify-center text-xl font-bold shadow-lg shadow-blue-500/20">
              ☁️
            </div>
            <h3 class="text-xl font-bold text-slate-900">Облачная доступность 24/7</h3>
            <p class="text-sm text-slate-600 leading-relaxed">
              Работа прямо в браузере с любого устройства без установки и серверов. Филиалы и сотрудники видят актуальные данные одновременно в реальном времени.
            </p>
          </div>

          <!-- Преимущество 2 -->
          <div class="p-8 rounded-3xl bg-slate-50 border border-slate-200/80 hover:border-blue-300 hover:shadow-xl transition space-y-4">
            <div class="w-12 h-12 rounded-2xl bg-emerald-600 text-white flex items-center justify-center text-xl font-bold shadow-lg shadow-emerald-500/20">
              ⚡
            </div>
            <h3 class="text-xl font-bold text-slate-900">Умное автораспределение</h3>
            <p class="text-sm text-slate-600 leading-relaxed">
              Алгоритмы сами рассчитывают потребность точек по истории продаж и распределяют товар по приоритетам («горит», «нужно», «можно») с учетом сроков годности (FEFO).
            </p>
          </div>

          <!-- Преимущество 3 -->
          <div class="p-8 rounded-3xl bg-slate-50 border border-slate-200/80 hover:border-blue-300 hover:shadow-xl transition space-y-4">
            <div class="w-12 h-12 rounded-2xl bg-indigo-600 text-white flex items-center justify-center text-xl font-bold shadow-lg shadow-indigo-500/20">
              📊
            </div>
            <h3 class="text-xl font-bold text-slate-900">Автоматизация расчетов</h3>
            <p class="text-sm text-slate-600 leading-relaxed">
              Забудьте про ошибки ручного счёта в Excel. Выплаты водителям, комиссии диспетчеров, сверки, факторинг и зарплаты формируются автоматически.
            </p>
          </div>

          <!-- Преимущество 4 -->
          <div class="p-8 rounded-3xl bg-slate-50 border border-slate-200/80 hover:border-blue-300 hover:shadow-xl transition space-y-4">
            <div class="w-12 h-12 rounded-2xl bg-amber-500 text-white flex items-center justify-center text-xl font-bold shadow-lg shadow-amber-500/20">
              🛡️
            </div>
            <h3 class="text-xl font-bold text-slate-900">Устойчивость к сбоям связи</h3>
            <p class="text-sm text-slate-600 leading-relaxed">
              Кассовые терминалы продолжают работать даже при временном обрыве интернета. При восстановлении связи все данные автоматически синхронизируются и выравниваются.
            </p>
          </div>

          <!-- Преимущество 5 -->
          <div class="p-8 rounded-3xl bg-slate-50 border border-slate-200/80 hover:border-blue-300 hover:shadow-xl transition space-y-4">
            <div class="w-12 h-12 rounded-2xl bg-purple-600 text-white flex items-center justify-center text-xl font-bold shadow-lg shadow-purple-500/20">
              📉
            </div>
            <h3 class="text-xl font-bold text-slate-900">Минимум списаний и потерь</h3>
            <p class="text-sm text-slate-600 leading-relaxed">
              Строгий контроль сроков годности, отслеживание каждой партии от приемки до продажи и предотвращение образования «мертвых» запасов на складах.
            </p>
          </div>

          <!-- Преимущество 6 -->
          <div class="p-8 rounded-3xl bg-slate-50 border border-slate-200/80 hover:border-blue-300 hover:shadow-xl transition space-y-4">
            <div class="w-12 h-12 rounded-2xl bg-rose-600 text-white flex items-center justify-center text-xl font-bold shadow-lg shadow-rose-500/20">
              📈
            </div>
            <h3 class="text-xl font-bold text-slate-900">Прозрачная отчетность и KPI</h3>
            <p class="text-sm text-slate-600 leading-relaxed">
              Руководство видит актуальные дашборды, показатели эффективности сотрудников, финансовые результаты и экспорт в Excel/PDF в один клик.
            </p>
          </div>

        </div>
      </div>
    </section>

    <!-- Секция: Боли и Решения бизнеса -->
    <section id="problems" class="py-24">
      <div class="max-w-7xl mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16 space-y-4">
          <h2 class="text-3xl lg:text-4xl font-extrabold text-slate-900 mt-6 whitespace-nowrap mx-auto w-fit">С чем сталкивается аптечный бизнес</h2>
          <p class="text-slate-600">Мы устраняем хаос, человеческий фактор и финансовые потери на всех уровнях: от закупа до касс</p>
        </div>

        <div class="grid md:grid-cols-2 gap-8">

          <!-- Ситуация 1: Склад -->
          <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between space-y-6">
            <div class="space-y-4">
              <span class="inline-block mb-3 px-3 py-1 bg-rose-50 text-rose-600 text-xs font-bold rounded-full">Проблема на складе</span>
              <p class="text-slate-600 text-sm leading-relaxed italic">
                «Склады стоят, сотрудники завалены посылками, никто не понимает, где что находится. Возникают неконтролируемые списания, пересортица и правки документов задним числом…»
              </p>
              <div class="pt-4 border-t border-slate-100 space-y-2">
                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Наше решение:</span>
                <p class="text-sm text-slate-700 leading-relaxed">
                  Организуем движение товаров по ячейкам с точностью до единицы. Перекрестные проверки и жесткое логирование исключают ошибки, путаницу и несанкционированные правки.
                </p>
              </div>
            </div>
          </div>

          <!-- Ситуация 2: Инвентаризация -->
          <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between space-y-6">
            <div class="space-y-4">
              <span class="inline-block mb-3 px-3 py-1 bg-rose-50 text-rose-600 text-xs font-bold rounded-full">Инвентаризация без простоев</span>
              <p class="text-slate-600 text-sm leading-relaxed italic">
                «Нужно делать инвентаризацию, но останавливать работу склада или точек никак нельзя...»
              </p>
              <div class="pt-4 border-t border-slate-100 space-y-2">
                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Наше решение:</span>
                <p class="text-sm text-slate-700 leading-relaxed">
                  Система позволяет проводить <b>непрерывную инвентаризацию</b> прямо на работающем складе, гарантируя абсолютную точность результатов и исключая пересортицу.
                </p>
              </div>
            </div>
          </div>

          <!-- Ситуация 3: Распределение товара -->
          <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between space-y-6">
            <div class="space-y-4">
              <span class="inline-block mb-3 px-3 py-1 bg-rose-50 text-rose-600 text-xs font-bold rounded-full">Распределение и просрочка</span>
              <p class="text-slate-600 text-sm leading-relaxed italic">
                «В одних точках товар пылится, в других его ждут месяцы. Никто не знает точно объемы просрочки и дефицита...»
              </p>
              <div class="pt-4 border-t border-slate-100 space-y-2">
                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Наше решение:</span>
                <p class="text-sm text-slate-700 leading-relaxed">
                  Алгоритм с точностью до пачки знает потребности точек, автоматически распределяет товар по приоритетам, строго контролируя партии и сроки годности (FEFO) для минимизации убытков.
                </p>
              </div>
            </div>
          </div>

          <!-- Ситуация 4: Нечеткое сопоставление прайсов -->
          <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between space-y-6">
            <div class="space-y-4">
              <span class="inline-block mb-3 px-3 py-1 bg-rose-50 text-rose-600 text-xs font-bold rounded-full">Хаос в прайсах поставщиков</span>
              <p class="text-slate-600 text-sm leading-relaxed italic">
                «У каждого поставщика свое описание, единых идентификаторов нет. Сотрудники вручную сопоставляют тысячи строк...»
              </p>
              <div class="pt-4 border-t border-slate-100 space-y-2">
                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Наше решение:</span>
                <p class="text-sm text-slate-700 leading-relaxed">
                  Интеллектуальная система семантического сопоставления распознает индустриальные сокращения и идиомы. Большинство позиций связываются автоматически, а решения по спорным случаям система запоминает навсегда.
                </p>
              </div>
            </div>
          </div>

          <!-- Ситуация 5: Ошибки в базах и департаментах -->
          <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between space-y-6">
            <div class="space-y-4">
              <span class="inline-block mb-3 px-3 py-1 bg-rose-50 text-rose-600 text-xs font-bold rounded-full">Рассинхрон департаментов</span>
              <p class="text-slate-600 text-sm leading-relaxed italic">
                «Документы из логистики и бухгалтерии не бьются между собой, полно ошибок, суммы и количества расходятся...»
              </p>
              <div class="pt-4 border-t border-slate-100 space-y-2">
                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Наше решение:</span>
                <p class="text-sm text-slate-700 leading-relaxed">
                  Интеллектуальный кросс-анализ данных и протоколов находит истину, автоматически выявляет неточности на ранних стадиях и исключает человеческие ошибки операторов.
                </p>
              </div>
            </div>
          </div>

          <!-- Ситуация 6: Неквалифицированный персонал -->
          <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between space-y-6">
            <div class="space-y-4">
              <span class="inline-block mb-3 px-3 py-1 bg-rose-50 text-rose-600 text-xs font-bold rounded-full">Неквалифицированный персонал</span>
              <p class="text-slate-600 text-sm leading-relaxed italic">
                «Текучка кадров, невнимательность, сотрудники игнорируют правила и ошибаются, обучение занимает месяцы...»
              </p>
              <div class="pt-4 border-t border-slate-100 space-y-2">
                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Наше решение:</span>
                <p class="text-sm text-slate-700 leading-relaxed">
                  Система не требует долгого обучения — она сама контролирует действия сотрудника. Сделать что-то неправильно физически не получится, а любые действия прозрачно журналируются.
                </p>
              </div>
            </div>
          </div>


          <!-- Ситуация 7: Аналитика и отчеты в реальном времени -->
          <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between space-y-6">
            <div class="space-y-4">
              <span class="inline-block mb-3 px-3 py-1 bg-rose-50 text-rose-600 text-xs font-bold rounded-full">Задержки в аналитике</span>
              <p class="text-slate-600 text-sm leading-relaxed italic">
                «Построение отчетов занимает часы, менеджмент живет в прошлом и не знает реального положения дел. Сторно и операционные ошибки — постоянная головная боль...»
              </p>
              <div class="pt-4 border-t border-slate-100 space-y-2">
                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Наше решение:</span>
                <p class="text-sm text-slate-700 leading-relaxed">
                  Расчет остатков и ключевых показателей происходит в реальном времени. Сторно и правки прошлых периодов обрабатываются автоматически и мгновенно отражаются в отчетах. Руководство может моделировать сценарии «а что если...» и быстро находить причины изменений.
                </p>
              </div>
            </div>
          </div>

          <!-- Ситуация 8: Интеграция с «зоопарком» систем -->
          <div class="bg-white rounded-3xl p-8 border border-slate-200 shadow-sm hover:shadow-xl transition flex flex-col justify-between space-y-6">
            <div class="space-y-4">
              <span class="inline-block mb-3 px-3 py-1 bg-rose-50 text-rose-600 text-xs font-bold rounded-full">Совместимость с имеющимся софтом</span>
              <p class="text-slate-600 text-sm leading-relaxed italic">
                «Вложены миллионы в текущие ERP, CRM, WMS, TMS, но они плохо дружат между собой. Синхронизация сбоит, а сотрудники ведут документы в нескольких системах одновременно...»
              </p>
              <div class="pt-4 border-t border-slate-100 space-y-2">
                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Наше решение:</span>
                <p class="text-sm text-slate-700 leading-relaxed">
                  Мы не заставляем отказываться от существующего софта. Система строит надежные мосты и интеграции к имеющимся решениям, объединяя данные и отдавая их бизнесу в удобном виде.
                </p>
              </div>
            </div>
          </div>

            </div>
          </div>
    </section>

    <!-- Секция заявки -->
    <section id="contact" class="min-h-screen flex items-center py-24 max-w-3xl mx-auto px-6">
      <div class="bg-white rounded-3xl p-8 lg:p-12 border border-slate-200 shadow-xl">
        <div class="text-center space-y-3 mb-10">
          <h2 class="text-3xl font-extrabold text-slate-900">Обсудим ваш проект?</h2>
          <p class="text-slate-600">Оставьте заявку, и наш архитектор свяжется с вами для бесплатной консультации</p>
        </div>

        <div v-if="isSubmitted" class="p-6 bg-emerald-50 border border-emerald-200 rounded-2xl text-center space-y-2">
          <div class="text-3xl">🎉</div>
          <h3 class="font-bold text-emerald-900 text-lg">Заявка успешно отправлена!</h3>
          <p class="text-sm text-emerald-700">Мы свяжемся с вами в течение 15 минут.</p>
        </div>

        <form v-else @submit.prevent="submitForm" class="space-y-6">
          <div class="grid sm:grid-cols-2 gap-6">
            <div>
              <label class="block text-sm font-semibold text-slate-700 mb-2">Ваше имя *</label>
              <input type="text" v-model="form.name" required placeholder="Имя" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500">
            </div>
            <div>
              <label class="block text-sm font-semibold text-slate-700 mb-2">Телефон *</label>
              <input type="tel" v-model="form.phone" required placeholder="+998 (__) ___-__-__" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500">
            </div>
          </div>
          <div>
            <label class="block text-sm font-semibold text-slate-700 mb-2">Название компании</label>
            <input type="text" v-model="form.company" placeholder="Компания" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500">
          </div>
          <div>
            <label class="block text-sm font-semibold text-slate-700 mb-2">Ваш вопрос</label>
            <textarea rows="4" v-model="form.message" placeholder="..." class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500"></textarea>
          </div>
          <button type="submit" :disabled="isSubmitting" class="w-full py-4 rounded-xl bg-blue-600 text-white font-bold hover:bg-blue-700 transition shadow-lg shadow-blue-500/30 disabled:opacity-60 disabled:cursor-not-allowed">
            {{ isSubmitting ? 'Отправка...' : 'Отправить заявку' }}
          </button>
          <p v-if="submitError" class="text-sm text-rose-600 text-center">
            Не удалось отправить заявку. Попробуйте ещё раз или напишите нам на почту.
          </p>
        </form>
      </div>
    </section>

    <!-- Подвал -->
    <footer class="bg-slate-900 text-slate-400 py-12 border-t border-slate-800">
      <div class="max-w-7xl mx-auto px-6 flex flex-col sm:flex-row items-center justify-between gap-6">
        <div class="flex items-center space-x-3">
          <div class="w-8 h-8 rounded-lg bg-blue-600 flex items-center justify-center text-white font-bold text-sm">
            DI
          </div>
          <span class="text-white font-bold">DigitaIAM</span>
        </div>
        <p class="text-sm">© 2026 Все права защищены. Разработка корпоративного софта.</p>
      </div>
    </footer>

    <!-- Полноэкранный просмотр изображения -->
    <div v-if="activeImage" class="fixed inset-0 z-50 bg-slate-900/80 backdrop-blur-sm flex items-center justify-center p-4" @click="closeImage">
      <div class="relative max-w-7xl w-full max-h-[90vh]">
        <button @click="closeImage" class="absolute -top-4 -right-4 w-10 h-10 rounded-full bg-white flex items-center justify-center text-slate-600 hover:bg-slate-200 font-bold shadow-lg z-10">
          ✕
        </button>
        <img :src="activeImage" alt="Просмотр изображения" class="w-full h-auto max-h-[90vh] object-contain rounded-2xl shadow-2xl" />
      </div>
    </div>

    <!-- Полноэкранный просмотр изображения -->
    <div v-if="activeImage" class="fixed inset-0 z-50 bg-slate-900/80 backdrop-blur-sm flex items-center justify-center p-4" @click="closeImage">
      <div class="relative max-w-7xl w-full max-h-[90vh]">
        <button @click="closeImage" class="absolute -top-4 -right-4 w-10 h-10 rounded-full bg-white flex items-center justify-center text-slate-600 hover:bg-slate-200 font-bold shadow-lg z-10">
          ✕
        </button>
        <img :src="activeImage" alt="Просмотр изображения" class="w-full h-auto max-h-[90vh] object-contain rounded-2xl shadow-2xl" />
      </div>
    </div>

    <!-- Модальное окно с деталями проекта -->
    <div v-if="activeProject" class="fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm flex items-center justify-center p-4" @click.self="closeProject">
      <div class="bg-white rounded-3xl max-w-7xl w-full p-8 lg:p-10 max-h-[90vh] overflow-y-auto space-y-8 relative shadow-2xl">
        <button @click="closeProject" class="sticky top-0 float-right -mt-2 -mr-2 w-10 h-10 rounded-full bg-slate-100 flex items-center justify-center text-slate-500 hover:bg-slate-200 font-bold z-10">
          ✕
        </button>

        <!-- ===================== TRUCKS.LAND ===================== -->
        <div v-if="activeProject === 'trucks'">
          <span class="px-3 py-1 bg-blue-50 text-blue-600 text-xs font-bold rounded-full">Логистика & TMS</span>
          <h2 class="text-2xl lg:text-3xl font-extrabold text-slate-900 mt-3">Trucks.land — система управления грузоперевозками</h2>
          <p class="text-slate-600 mt-3 text-sm leading-relaxed">
            Облачная система (TMS) для управления транспортной компанией: заказы и перевозки, отслеживание статусов в реальном времени,
            взаиморасчёты с владельцами тягачей и диспетчерами, а также прозрачная отчётность. Вся работа — в браузере, без установки, с любого устройства.
          </p>

          <div class="space-y-8 mt-8">
            <div>
              <h4 class="font-bold text-slate-900">Что система даёт клиенту</h4>
              <ul class="mt-2 space-y-1.5 text-sm text-slate-700 list-disc list-inside">
                <li>Единое рабочее место для диспетчеров, менеджеров, владельцев тягачей и руководства.</li>
                <li>Автоматизация расчётов вместо таблиц Excel: выплаты, комиссии, сверки и отчёты формируются автоматически.</li>
                <li>Контроль каждой перевозки: от создания заказа до его закрытия и оплаты.</li>
                <li>Работа в реальном времени: несколько организаций и сотрудников видят актуальные данные одновременно.</li>
              </ul>
            </div>

            <div>
              <h4 class="font-bold text-slate-900">Для кого</h4>
              <p class="mt-2 text-sm text-slate-700 leading-relaxed">
                Для транспортных и логистических компаний: диспетчеры, менеджеры по перевозкам, владельцы тягачей, бухгалтерия и руководство.
                Начать работу просто — достаточно создать учётную запись и организацию.
              </p>
            </div>

            <div>
              <h4 class="font-bold text-slate-900">Ключевые возможности</h4>
              <div class="mt-3 grid sm:grid-cols-2 gap-4 text-sm">
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-blue-700">Заказы и перевозки</p>
                  <p class="text-slate-700">Создание и редактирование заказов: номер, брокер, вес, мили, стоимость, ставки. Гибкая система статусов и этапов. Комментарии, файлы, история изменений, поиск похожих загрузок и контроль согласований.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-blue-700">Отслеживание в реальном времени</p>
                  <p class="text-slate-700">Живой дашборд перевозок: текущий статус каждого заказа, время, цветовое выделение этапов. Фильтры по заказам, транспорту, водителям и статусам.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-blue-700">Справочники</p>
                  <p class="text-slate-700">Брокеры, владельцы тягачей, водители, транспорт, организации, сотрудники. Страховки по транспортному средству и водителю. Схемы комиссий и настройка тарифов.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-blue-700">Платежи и взаиморасчёты</p>
                  <p class="text-slate-700">Расчёты с владельцами тягачей, выплаты диспетчерам и сотрудникам. Штрафы, начисления, отсутствия, история найма. Контроль неоплаченных заказов и дебиторской задолженности.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-blue-700">Отчёты и журналы</p>
                  <p class="text-slate-700">Отчёты по владельцам и сотрудникам/диспетчерам, эффективность диспетчеров, экспорт в Excel и PDF, отправка документов на почту. Факторинг и экспресс-выплаты (quick pays).</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-blue-700">Кадры, задачи и безопасность</p>
                  <p class="text-slate-700">Задачи и рабочий контроль, учёт рабочего времени, расчёт зарплаты. Мультитенантность и разграничение прав доступа между ролями.</p>
                </div>
              </div>
            </div>

            <div>
              <h4 class="font-bold text-slate-900">Преимущества</h4>
              <ul class="mt-2 space-y-1.5 text-sm text-slate-700 list-disc list-inside">
                <li><b>Облако</b> — доступ из любой точки мира, не нужна установка и серверы.</li>
                <li><b>Прозрачность</b> — каждый этап и сумма видны, ошибки ручного пересчёта исключены.</li>
                <li><b>Масштабируемость</b> — подходит и небольшой компании, и сети организаций.</li>
                <li><b>Экономия времени</b> — автоматические сверки, готовые отчёты и экспорт в Excel/PDF.</li>
              </ul>
            </div>
          </div>
        </div>

        <!-- ===================== DORI.PRO ===================== -->
        <div v-if="activeProject === 'dori'">
          <span class="px-3 py-1 bg-emerald-50 text-emerald-600 text-xs font-bold rounded-full">Фармацевтика & Ритейл</span>
          <h2 class="text-2xl lg:text-3xl font-extrabold text-slate-900 mt-3">Dori.pro — система управления аптечной сетью</h2>
          <p class="text-slate-600 mt-3 text-sm leading-relaxed">
            Единая система управления аптечной сетью, объединяющая работу кассира, склада, закупок, логистики и руководства в одном цифровом контуре.
            Все данные — остатки, продажи, движения товаров, платежи — хранятся в облаке и доступны в реальном времени: от кассы до рабочего места директора.
          </p>

          <div class="space-y-8 mt-8">
            <div>
              <h4 class="font-bold text-slate-900">Проблема и решение</h4>
              <div class="mt-2 grid sm:grid-cols-2 gap-4 text-sm">
                <div class="p-4 rounded-xl bg-rose-50 border border-rose-200 space-y-1.5">
                  <p class="font-semibold text-rose-700">Без единой системы</p>
                  <p class="text-slate-700">Продажи и остатки «на бумаге», закупки «вслепую», распределение товара вручную, нет контроля сроков годности, отчёты собираются с опозданием.</p>
                </div>
                <div class="p-4 rounded-xl bg-emerald-50 border border-emerald-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">С Dori.pro</p>
                  <p class="text-slate-700">Каждое движение товара фиксируется автоматически. Потребность аптек рассчитывается по фактическим продажам, а товар распределяется сам с учётом спроса и сроков годности.</p>
                </div>
              </div>
            </div>

            <div>
              <h4 class="font-bold text-slate-900">Ключевое преимущество: анализ потребностей и автораспределение</h4>
              <div class="mt-3 space-y-4 text-sm">
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-slate-900">Анализ потребностей аптек</p>
                  <p class="text-slate-700">Расчёт потребности каждой аптеки по истории продаж за 90 дней: прогноз спроса на 7 / 14 / 21 день («горит» / «нужно» / «можно»), потребность на 28 дней, дефицит и избыток, автоматический объём заказа у поставщика с учётом остатков и категории товара.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-slate-900">Автоматическое распределение по аптекам</p>
                  <p class="text-slate-700">Система сама предлагает, кому и сколько отгрузить: три уровня приоритета, учёт сроков годности, бережное отношение к партиям, исключение повторного распределения, приоритизация аптек с высоким спросом. По результатам — готовые листы отгрузки и задачи для сборщиков.</p>
                </div>
              </div>
            </div>

            <div>
              <h4 class="font-bold text-slate-900">Возможности по модулям</h4>
              <div class="mt-3 grid sm:grid-cols-2 gap-4 text-sm">
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">POS-касса (продажи)</p>
                  <p class="text-slate-700">Быстрая продажа по штрихкоду, списание по FEFO, продажа в долг, карантин товара, смешанные платежи (наличные + карта + QR), печать чека 58 мм, X/Z-отчёты и закрытие бизнес-дня.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">Платёжные интеграции</p>
                  <p class="text-slate-700">Фискализация через Arca (UzCard), карты Humo/UzCard/SmartOne, QR-платежи Click, Payme, Uzum FastPay. Устойчивость к сбоям без двойного списания (RRN).</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">Склад и запасы</p>
                  <p class="text-slate-700">Приёмка по партиям, автоматическое распределение и перемещения, ячейки/полки, инвентаризации, контроль отрицательных остатков и сроков годности, полный журнал движений.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">Заказы и закупки</p>
                  <p class="text-slate-700">Заявки аптек, автоматический расчёт объёма к заказу, заказы поставщикам, договоры и счета, сравнение накладной и поступления, анализ поставщиков по срокам, ценам и KPI.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">Логистика и мобильные приложения</p>
                  <p class="text-slate-700">Сборка и упаковка по листам распределения, отслеживание посылок, приложение водителя, приложение менеджера аптеки: распределение по ячейкам со сканированием камерой.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">Отчёты и аналитика</p>
                  <p class="text-slate-700">KPI по закупкам, продажам, контролёрам, сборщикам и поставщикам. Отчёты по товарообороту, остаткам, продажам по фармацевтам и категориям, прогноз спроса, бонусы и дашборд руководства.</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">Задачи и процессы</p>
                  <p class="text-slate-700">Housekeeping/задачи, проекты и шаблоны типовых процессов, маркетинг (договоры, план продаж, акции и скидки).</p>
                </div>
                <div class="p-4 rounded-xl bg-slate-50 border border-slate-200 space-y-1.5">
                  <p class="font-semibold text-emerald-700">Контроль GPP и справочники</p>
                  <p class="text-slate-700">Журнал замеров температуры в зонах хранения, единый товарный каталог, справочники, роли и права доступа, настройка состава сети и терминалов.</p>
                </div>
              </div>
            </div>

            <div>
              <h4 class="font-bold text-slate-900">Как это устроено</h4>
              <ul class="mt-2 space-y-1.5 text-sm text-slate-700 list-disc list-inside">
                <li>Облачная база данных (HA / PostgreSQL / Supabase) с резервным копированием.</li>
                <li>Работа в реальном времени и при слабой связи: касса работает даже при сбоях интернета.</li>
                <li>Безопасность по ролям и шифрование данных, автоматические удалённые обновления.</li>
                <li>Поддержка оборудования: штрих-сканеры, термопринтеры 58 мм, фискальные и банковские терминалы, весовые платформы.</li>
              </ul>
            </div>

            <div>
              <h4 class="font-bold text-slate-900">Выгоды для бизнеса</h4>
              <ul class="mt-2 space-y-1.5 text-sm text-slate-700 list-disc list-inside">
                <li>Скорость кассы — меньше очередей, больше оборот.</li>
                <li>Точные остатки в любой момент по любой аптеке — минимум потерь.</li>
                <li>Умное распределение товара — товар попадает туда, где его ждут.</li>
                <li>Меньше списаний благодаря распределению по срокам годности.</li>
                <li>Прозрачная отчётность и единые процессы для всех точек сети.</li>
              </ul>
            </div>

            <div>
              <h4 class="font-bold text-slate-900">Внедрение и поддержка</h4>
              <p class="mt-2 text-sm text-slate-700 leading-relaxed">
                Анализ процессов и настройка под нужды бизнеса, оборудование касс и терминалов, обучение персонала и дальнейшее сопровождение:
                обновления, обучение новых сотрудников, добавление функционала по мере роста.
              </p>
            </div>
          </div>
        </div>

        <button @click="closeProject" class="w-full py-3 rounded-xl bg-slate-900 text-white font-semibold hover:bg-slate-800 transition">
          Закрыть
        </button>
      </div>
    </div>

  </div>
</template>