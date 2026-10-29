<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dukan Sathi - Gram Hisab (450g Cheeni) + Saved Google Sheet</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Inter', sans-serif; }
  </style>
</head>
<body class="bg-amber-50 min-h-screen text-slate-800 p-3 sm:p-5">

  <div class="max-w-xl mx-auto space-y-4">
    <!-- Header with Custom Shop Name, Summaries & Saved Google Sheet Direct Button -->
    <header class="bg-amber-600 text-white rounded-2xl p-4 sm:p-5 shadow-md space-y-3">
      <div class="flex flex-wrap items-center justify-between gap-3">
        <div>
          <div class="flex flex-wrap items-center gap-2 mb-1">
            <span class="inline-block text-xs font-semibold uppercase tracking-wider bg-amber-700/80 px-2.5 py-1 rounded-full">
              Gram Hisab + Google Sheet Connected
            </span>
            <span id="autoSaveBadge" class="inline-flex items-center gap-1 text-xs font-medium bg-emerald-700/90 text-emerald-100 px-2.5 py-1 rounded-full">
              💾 Auto-Save Chalu
            </span>
          </div>
          <div class="flex flex-wrap items-center gap-2">
            <h1 id="shopNameHeading" class="text-2xl font-bold">🏪 Dukan Sathi</h1>
            <button
              type="button"
              id="editShopNameBtn"
              class="text-xs bg-amber-700 hover:bg-amber-800 text-amber-100 px-2.5 py-1 rounded-lg border border-amber-400/40 transition"
            >
              ✏️ Dukan Naam Badlein
            </button>
          </div>
          <p class="text-amber-100 text-xs sm:text-sm">ग्राम/किलो हिसाब (जैसे 450g चीनी), उधारी खाता और आपकी Google Sheet</p>
        </div>

        <!-- 4 Live Counters: Saman, Bikri, Udhari, Profit/Loss -->
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 w-full sm:w-auto">
          <div class="text-right bg-amber-700/60 px-3 py-1.5 rounded-xl">
            <div class="text-[11px] text-amber-100">Kul Saman</div>
            <div id="totalItemsCount" class="text-base font-bold">0</div>
          </div>
          <div class="text-right bg-emerald-700/90 px-3 py-1.5 rounded-xl border border-emerald-400/40">
            <div class="text-[11px] text-emerald-100">💵 Aaj ki Bikri</div>
            <div id="todaySalesCount" class="text-base font-bold">₹0</div>
          </div>
          <div class="text-right bg-rose-700/90 px-3 py-1.5 rounded-xl border border-rose-300/40">
            <div class="text-[11px] text-rose-100">📒 Kul Udhari Baki</div>
            <div id="totalUdhariCount" class="text-base font-bold">₹0</div>
          </div>
          <div class="text-right bg-indigo-800/90 px-3 py-1.5 rounded-xl border border-indigo-300/40">
            <div class="text-[11px] text-indigo-100">📊 Profit / Loss</div>
            <div id="headerNetProfitCount" class="text-base font-bold">+₹0</div>
          </div>
        </div>
      </div>

      <!-- Direct Top Banner to Open User's Saved Google Sheet in Google Drive -->
      <div class="bg-emerald-800/95 border border-emerald-400/50 rounded-xl p-3 flex flex-col sm:flex-row sm:items-center justify-between gap-2">
        <div class="text-xs text-emerald-50">
          <strong>✅ आपकी Google Sheet कहाँ सेव है?</strong> आपके Google Drive में <em>"Dukan Sathi - Profit, Udhari &amp; Loss Hisab"</em> नाम से सेव है।
        </div>
        <a
          href="https://docs.google.com/spreadsheets/d/1TUnY1im5H2oKU6B8fboZrqevnXuHyAZFWYbe9T8R5KU/edit"
          target="_blank"
          rel="noopener noreferrer"
          id="topSavedSheetLink"
          class="bg-white hover:bg-emerald-50 text-emerald-900 font-bold text-xs px-3.5 py-2 rounded-lg shadow-sm transition text-center shrink-0"
        >
          📂 अपनी सेव Google Sheet खोलें ↗️
        </a>
      </div>

      <!-- Inline Edit Shop Name Box -->
      <form id="shopNameForm" class="hidden bg-amber-700/90 p-3 rounded-xl border border-amber-400/50 flex flex-col sm:flex-row gap-2">
        <input
          id="shopNameInput"
          type="text"
          placeholder="Apni dukan ka naam likhein (jaise: Aditya Kirana Store)"
          required
          class="flex-1 px-3 py-2 rounded-lg text-slate-900 text-sm font-semibold focus:outline-none"
        />
        <div class="flex items-center gap-2">
          <button
            type="submit"
            id="saveShopNameBtn"
            class="bg-emerald-600 hover:bg-emerald-500 text-white font-semibold px-3.5 py-2 rounded-lg text-xs sm:text-sm transition"
          >
            💾 Naam Save Karein
          </button>
          <button
            type="button"
            id="cancelShopNameBtn"
            class="bg-amber-800 hover:bg-amber-900 text-amber-100 font-medium px-3 py-2 rounded-lg text-xs sm:text-sm transition"
          >
            Radd
          </button>
        </div>
      </form>
    </header>

    <!-- Navigation Tabs (6 Tabs) -->
    <div class="grid grid-cols-2 sm:grid-cols-3 gap-1.5 bg-amber-100/90 p-1.5 rounded-2xl border border-amber-200">
      <button
        type="button"
        id="tabRateListBtn"
        class="py-2.5 px-2 rounded-xl font-semibold text-xs sm:text-sm transition flex items-center justify-center gap-1 text-amber-900 hover:bg-white/60"
      >
        <span>📋 Saman Rate List</span>
      </button>
      <button
        type="button"
        id="tabCalcBtn"
        class="py-2.5 px-2 rounded-xl font-semibold text-xs sm:text-sm transition flex items-center justify-center gap-1 bg-white text-amber-700 shadow-sm"
      >
        <span>🧮 Wajan Hisab</span>
      </button>
      <button
        type="button"
        id="tabBillBtn"
        class="py-2.5 px-2 rounded-xl font-semibold text-xs sm:text-sm transition flex items-center justify-center gap-1 text-amber-900 hover:bg-white/60"
      >
        <span>🧾 Parchi Bill</span>
        <span id="billCountBadge" class="bg-amber-600 text-white text-xs font-bold px-1.5 py-0.5 rounded-full">0</span>
      </button>
      <button
        type="button"
        id="tabUdhariBtn"
        class="py-2.5 px-2 rounded-xl font-semibold text-xs sm:text-sm transition flex items-center justify-center gap-1 text-amber-900 hover:bg-white/60"
      >
        <span>📒 Udhari Khata</span>
        <span id="udhariCustomerBadge" class="bg-rose-100 text-rose-700 text-xs font-bold px-2 py-0.5 rounded-full">0</span>
      </button>
      <button
        type="button"
        id="tabMandiBtn"
        class="py-2.5 px-2 rounded-xl font-semibold text-xs sm:text-sm transition flex items-center justify-center gap-1 text-amber-900 hover:bg-white/60"
      >
        <span>📦 Mandi List</span>
        <span id="mandiCountBadge" class="bg-indigo-100 text-indigo-700 text-xs font-bold px-2 py-0.5 rounded-full">0</span>
      </button>
      <button
        type="button"
        id="tabSheetBtn"
        class="py-2.5 px-2 rounded-xl font-semibold text-xs sm:text-sm transition flex items-center justify-center gap-1 text-amber-900 hover:bg-white/60"
      >
        <span>📊 Google Sheet Hisab</span>
      </button>
    </div>

    <!-- Toast Notification -->
    <div id="toastMessage" class="hidden bg-emerald-600 text-white text-sm font-medium px-4 py-2.5 rounded-xl shadow transition-all"></div>

    <!-- ========================================== -->
    <!-- VIEW 1: SAMAN RATE LIST                    -->
    <!-- ========================================== -->
    <div id="rateListView" class="hidden space-y-4">
      <section id="editRateBox" class="hidden bg-amber-50 rounded-2xl p-4 shadow-sm border-2 border-amber-400 space-y-3">
        <div class="flex items-center justify-between">
          <h3 class="text-sm font-bold text-amber-900 flex items-center gap-1.5">
            <span>✏️ Saman ka Naya Rate Likhein:</span>
            <span id="editItemNameLabel" class="underline"></span>
          </h3>
          <button
            type="button"
            id="cancelEditRateBtn"
            class="text-xs text-slate-500 hover:text-slate-800 font-semibold px-2 py-1 rounded-lg"
          >
            Band Karein
          </button>
        </div>
        <form id="editRateForm" class="flex flex-col sm:flex-row gap-2">
          <input
            id="editRateInput"
            type="number"
            min="1"
            step="any"
            placeholder="Naya Rate (₹)"
            required
            class="flex-1 px-3.5 py-2 rounded-xl border border-amber-300 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm font-semibold"
          />
          <button
            type="submit"
            id="confirmEditRateBtn"
            class="bg-emerald-600 hover:bg-emerald-700 text-white font-semibold px-4 py-2 rounded-xl text-sm transition"
          >
            💾 Naya Rate Save Karein
          </button>
        </form>
      </section>

      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-amber-100">
        <h2 class="text-base font-semibold text-slate-800 mb-3 flex items-center gap-2">
          <span>➕ Naya Saman aur Rate Jodein</span>
        </h2>
        <form id="addItemForm" class="grid grid-cols-1 sm:grid-cols-12 gap-2.5">
          <input
            id="itemNameInput"
            type="text"
            placeholder="Saman ka naam (jaise: Chawal, Maggi)"
            required
            class="sm:col-span-4 px-3 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm"
          />
          <input
            id="itemPriceInput"
            type="number"
            min="1"
            step="any"
            placeholder="Rate (₹)"
            required
            class="sm:col-span-2 px-3 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm"
          />
          <select
            id="itemUnitInput"
            class="sm:col-span-2 px-2 py-2.5 rounded-xl border border-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm"
          >
            <option value="kg">per kg</option>
            <option value="pkt">packet</option>
            <option value="ltr">per ltr</option>
            <option value="pc">piece</option>
          </select>
          <select
            id="itemCategoryInput"
            class="sm:col-span-2 px-2 py-2.5 rounded-xl border border-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm"
          >
            <option value="anaj">🌾 अनाज/दाल</option>
            <option value="tel">🛢️ तेल/मसाले</option>
            <option value="packet">🍪 पैकेट/चाय</option>
            <option value="sabun">🧼 साबुन/अन्य</option>
          </select>
          <button
            type="submit"
            id="saveItemBtn"
            class="sm:col-span-2 bg-amber-600 hover:bg-amber-700 active:bg-amber-800 text-white font-semibold py-2.5 px-3 rounded-xl text-sm transition"
          >
            Jodein
          </button>
        </form>
      </section>

      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-amber-100 space-y-4">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2">
          <div>
            <h2 class="text-base font-semibold text-slate-800">📋 Saman ki Rate List</h2>
            <p class="text-xs text-slate-400">Kisi bhi saman ke aage <strong>🧮 ग्राम हिसाब</strong> daba kar uska wajan daam dekhein</p>
          </div>
          <div class="relative flex-1 sm:max-w-xs">
            <input
              id="searchInput"
              type="text"
              placeholder="🔍 Saman khojein (type karein)..."
              class="w-full pl-3.5 pr-8 py-2 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm bg-slate-50"
            />
            <button
              type="button"
              id="clearSearchBtn"
              title="Clear search"
              class="hidden absolute right-2.5 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-600 text-xs font-bold px-1"
            >
              ✕
            </button>
          </div>
        </div>

        <div class="flex flex-wrap gap-1.5 pt-1">
          <button
            type="button"
            id="catAllBtn"
            class="text-xs bg-amber-600 text-white font-semibold px-3 py-1.5 rounded-full transition"
          >
            सभी सामान (All)
          </button>
          <button
            type="button"
            id="catAnajBtn"
            class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-full transition"
          >
            🌾 अनाज/दाल
          </button>
          <button
            type="button"
            id="catTelBtn"
            class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-full transition"
          >
            🛢️ तेल/मसाले
          </button>
          <button
            type="button"
            id="catPacketBtn"
            class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-full transition"
          >
            🍪 पैकेट/चाय
          </button>
          <button
            type="button"
            id="catSabunBtn"
            class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-full transition"
          >
            🧼 साबुन/अन्य
          </button>
        </div>

        <div id="itemsListContainer" class="divide-y divide-slate-100"></div>

        <div id="emptyState" class="hidden text-center py-8 text-slate-400 text-sm">
          Is category me koi saman nahi mila. Upar form se naya saman jodein!
        </div>
      </section>
    </div>

    <!-- ========================================== -->
    <!-- VIEW 2: SUPER CLEAR GRAM & WAJAN CALCULATOR-->
    <!-- ========================================== -->
    <div id="calcView" class="space-y-4">
      <!-- Box 1: Direct Rate (Rs/kg) x Grams Calculator (Solves Cheeni 45/kg -> 450g = Rs 20.25) -->
      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border-2 border-emerald-400 space-y-4">
        <div class="flex flex-wrap items-center justify-between gap-2 border-b border-emerald-100 pb-3">
          <div>
            <span class="inline-block text-[11px] font-bold uppercase bg-emerald-100 text-emerald-800 px-2.5 py-0.5 rounded-full mb-1">
              सबसे आसान ग्राम हिसाब (Direct Gram Calculator)
            </span>
            <h2 class="text-base sm:text-lg font-bold text-slate-900">
              ⚖️ 1 किलो का भाव लिखें और ग्राम लिखें — दाम तुरंत देखें!
            </h2>
            <p class="text-xs text-slate-600 mt-0.5">
              उदाहरण: <strong>चीनी ₹45 किलो</strong> है, तो <strong>450 ग्राम</strong> कितने का होगा? नीचे दोनों डिब्बों में लिखा है:
            </p>
          </div>
        </div>

        <!-- 2 Big Clear Inputs: 1 Kg Rate & Weight in Grams -->
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div class="bg-amber-50/70 p-3 rounded-xl border border-amber-200">
            <label for="directRateKgInput" class="block text-xs font-bold text-amber-950 mb-1">
              1️⃣ 1 किलो का भाव (₹ प्रति किलो):
            </label>
            <input
              id="directRateKgInput"
              type="number"
              min="1"
              step="any"
              value="45"
              class="w-full px-3.5 py-2.5 rounded-xl border-2 border-amber-400 bg-white text-lg font-extrabold text-slate-900 focus:outline-none focus:ring-2 focus:ring-amber-500"
            />
            <div class="text-[11px] text-amber-800 mt-1">जैसे चीनी = 45, चावल = 90, दाल = 140</div>
          </div>

          <div class="bg-emerald-50/70 p-3 rounded-xl border border-emerald-200">
            <label for="directGramsInput" class="block text-xs font-bold text-emerald-950 mb-1">
              2️⃣ कितना ग्राम तौलना है? (ग्राम में लिखें):
            </label>
            <input
              id="directGramsInput"
              type="number"
              min="1"
              step="any"
              value="450"
              class="w-full px-3.5 py-2.5 rounded-xl border-2 border-emerald-400 bg-white text-lg font-extrabold text-slate-900 focus:outline-none focus:ring-2 focus:ring-emerald-500"
            />
            <div class="text-[11px] text-emerald-800 mt-1">जैसे 450 ग्राम, 250 ग्राम, 100 ग्राम (1 किलो = 1000 ग्राम)</div>
          </div>
        </div>

        <!-- Quick Gram Buttons including 450g! -->
        <div class="space-y-1.5">
          <div class="text-xs font-bold text-slate-700">⚡ नीचे बटन दबाकर भी ग्राम चुन सकते हैं:</div>
          <div class="flex flex-wrap gap-1.5">
            <button type="button" id="direct50gBtn" class="text-xs bg-slate-100 hover:bg-amber-100 text-slate-800 border border-slate-300 font-semibold px-3 py-1.5 rounded-lg">50 ग्राम</button>
            <button type="button" id="direct100gBtn" class="text-xs bg-slate-100 hover:bg-amber-100 text-slate-800 border border-slate-300 font-semibold px-3 py-1.5 rounded-lg">100 ग्राम</button>
            <button type="button" id="direct200gBtn" class="text-xs bg-slate-100 hover:bg-amber-100 text-slate-800 border border-slate-300 font-semibold px-3 py-1.5 rounded-lg">200 ग्राम</button>
            <button type="button" id="direct250gBtn" class="text-xs bg-slate-100 hover:bg-amber-100 text-slate-800 border border-slate-300 font-semibold px-3 py-1.5 rounded-lg">250g (पाव भर)</button>
            <button type="button" id="direct450gBtn" class="text-xs bg-emerald-600 text-white font-bold px-3 py-1.5 rounded-lg shadow-sm">450 ग्राम (आपका उदाहरण)</button>
            <button type="button" id="direct500gBtn" class="text-xs bg-slate-100 hover:bg-amber-100 text-slate-800 border border-slate-300 font-semibold px-3 py-1.5 rounded-lg">500g (आधा किलो)</button>
            <button type="button" id="direct750gBtn" class="text-xs bg-slate-100 hover:bg-amber-100 text-slate-800 border border-slate-300 font-semibold px-3 py-1.5 rounded-lg">750g (पौने किलो)</button>
            <button type="button" id="direct1000gBtn" class="text-xs bg-slate-100 hover:bg-amber-100 text-slate-800 border border-slate-300 font-semibold px-3 py-1.5 rounded-lg">1000g (1 किलो)</button>
          </div>
        </div>

        <!-- Big Answer Box for Direct Gram Calculation -->
        <div class="bg-emerald-600 text-white rounded-2xl p-4 shadow-sm flex flex-col sm:flex-row sm:items-center justify-between gap-3">
          <div class="space-y-1">
            <div class="text-xs font-semibold text-emerald-100 uppercase">✅ पक्का हिसाब (Exact Price):</div>
            <div id="directFormulaExplain" class="text-sm font-medium text-white">
              ₹45/किलो के भाव से 450 ग्राम का दाम:
            </div>
            <div id="directStepByStepMath" class="text-xs text-emerald-100">
              गणित: (₹45 ÷ 1000 ग्राम) × 450 ग्राम = ₹20.25
            </div>
          </div>
          <div class="text-left sm:text-right bg-white text-emerald-800 px-4 py-2.5 rounded-xl shrink-0">
            <div id="directExactRupees" class="text-2xl sm:text-3xl font-extrabold">₹20.25</div>
            <div id="directRoundedRupees" class="text-xs font-bold text-emerald-600">ग्राहक से लें: ₹20</div>
          </div>
        </div>

        <!-- Reverse Calculation: Rupees to Grams ("Grahak bole ₹20 ki cheeni de do") -->
        <div class="bg-amber-50 border border-amber-200 rounded-xl p-3 flex flex-col sm:flex-row sm:items-center justify-between gap-2">
          <div class="flex items-center gap-2">
            <span class="text-xs font-bold text-amber-950">🔄 उल्टा हिसाब — अगर ग्राहक रुपये बोले (जैसे ₹20 में कितना ग्राम दें?):</span>
            <input
              id="reverseRupeeInput"
              type="number"
              min="1"
              step="any"
              value="20"
              class="w-24 px-2.5 py-1 rounded-lg border border-amber-400 bg-white text-sm font-bold text-slate-900"
            />
          </div>
          <div id="reverseGramsOutput" class="text-sm font-extrabold text-amber-900">
            👉 ₹20 में 444 ग्राम तौलें
          </div>
        </div>
      </section>

      <!-- Box 2: Select Item from Rate List & Send to Udhari -->
      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-amber-100 space-y-4">
        <div>
          <h2 class="text-base font-semibold text-slate-800 flex items-center gap-2">
            <span>📋 अपनी Rate List से सामान चुनकर हिसाब करें</span>
          </h2>
          <p class="text-xs text-slate-500 mt-0.5">
            यहाँ पहले से <strong>Cheeni (Sugar) ₹45/kg — 450 ग्राम = ₹20.25</strong> सेट है!
          </p>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-12 gap-3">
          <div class="sm:col-span-6">
            <label for="calcItemSelect" class="block text-xs font-medium text-slate-600 mb-1">1. Saman Chunein</label>
            <select
              id="calcItemSelect"
              class="w-full px-3.5 py-2.5 rounded-xl border border-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm font-medium"
            ></select>
          </div>

          <div class="sm:col-span-3">
            <label for="calcQtyInput" class="block text-xs font-medium text-slate-600 mb-1">2. Matra (Wajan/Ginti)</label>
            <input
              id="calcQtyInput"
              type="number"
              min="0"
              step="any"
              value="450"
              class="w-full px-3.5 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm font-semibold"
            />
          </div>

          <div class="sm:col-span-3">
            <label for="calcUnitMode" class="block text-xs font-medium text-slate-600 mb-1">3. Naap (Unit)</label>
            <select
              id="calcUnitMode"
              class="w-full px-3 py-2.5 rounded-xl border border-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm"
            >
              <option value="gram">ग्राम / मिली (gram / ml)</option>
              <option value="full">पूरा (kg / ltr / pkt / pc)</option>
            </select>
          </div>
        </div>

        <div class="space-y-1.5">
          <div class="text-xs font-medium text-slate-500">⚡ Jaldi Wajan Chunein (Quick Buttons):</div>
          <div class="flex flex-wrap gap-2">
            <button type="button" id="preset100gBtn" class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-lg transition">
              100g (छटाक)
            </button>
            <button type="button" id="preset250gBtn" class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-lg transition">
              250g (पाव भर)
            </button>
            <button type="button" id="preset450gBtn" class="text-xs bg-emerald-600 text-white font-semibold px-3 py-1.5 rounded-lg transition">
              450g (450 ग्राम)
            </button>
            <button type="button" id="preset500gBtn" class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-lg transition">
              500g (आधा किलो)
            </button>
            <button type="button" id="preset1kgBtn" class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-lg transition">
              1 किलो / 1 नग
            </button>
            <button type="button" id="preset2kgBtn" class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-lg transition">
              2 किलो / 2 नग
            </button>
            <button type="button" id="preset5kgBtn" class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-lg transition">
              5 किलो (पसेरी)
            </button>
          </div>
        </div>

        <div class="bg-emerald-50 border border-emerald-200 rounded-2xl p-4 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
          <div>
            <div class="text-xs font-medium text-emerald-800">Kul Daam (Total Price):</div>
            <div id="calcSummaryText" class="text-xs text-emerald-700 mt-0.5">450gram Cheeni (Sugar) (@ ₹45/kg)</div>
          </div>
          <div id="calcResultPrice" class="text-3xl font-bold text-emerald-700">₹20.25</div>
        </div>

        <div class="pt-2 border-t border-slate-100">
          <div class="text-xs font-semibold text-slate-700 mb-2">
            📒 Kya yeh saman Udhari me dena hai? Seedhe Khata me jodein:
          </div>
          <div class="flex flex-col sm:flex-row gap-2">
            <input
              id="calcCustomerInput"
              type="text"
              placeholder="Grahak ka naam likhein (jaise: Aditya Kumar)"
              class="flex-1 px-3.5 py-2 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500 text-sm"
            />
            <button
              type="button"
              id="sendCalcToUdhariBtn"
              class="bg-rose-600 hover:bg-rose-700 text-white font-semibold px-4 py-2 rounded-xl text-sm transition"
            >
              Udhari me Jodein
            </button>
          </div>
        </div>
      </section>
    </div>

    <!-- ========================================== -->
    <!-- VIEW 3: PARCHI BILL + AAJ KA GALLA         -->
    <!-- ========================================== -->
    <div id="billView" class="hidden space-y-4">
      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-amber-100 space-y-4">
        <div>
          <h2 class="text-base font-semibold text-slate-800 flex items-center gap-2">
            <span>🧾 Grahak ki Parchi / Bill Banayein</span>
          </h2>
          <p class="text-xs text-slate-500 mt-0.5">
            Saman jodein, phir <strong>Nagad Galla</strong> me save karein ya <strong>Udhari Khata</strong> me bhejein
          </p>
        </div>

        <form id="addToBillForm" class="grid grid-cols-1 sm:grid-cols-12 gap-2.5">
          <div class="sm:col-span-5">
            <select
              id="billItemSelect"
              class="w-full px-3 py-2.5 rounded-xl border border-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm font-medium"
            ></select>
          </div>
          <div class="sm:col-span-2">
            <input
              id="billQtyInput"
              type="number"
              min="0.01"
              step="any"
              value="1"
              placeholder="Matra"
              required
              class="w-full px-3 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm font-semibold"
            />
          </div>
          <div class="sm:col-span-3">
            <select
              id="billUnitMode"
              class="w-full px-2.5 py-2.5 rounded-xl border border-slate-200 bg-white focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm"
            >
              <option value="full">पूरा (kg/pkt/ltr)</option>
              <option value="gram">ग्राम/मिली (g/ml)</option>
            </select>
          </div>
          <button
            type="submit"
            id="addToBillBtn"
            class="sm:col-span-2 bg-amber-600 hover:bg-amber-700 text-white font-semibold py-2.5 px-3 rounded-xl text-sm transition"
          >
            + Parchi
          </button>
        </form>

        <!-- Bill Items Table / List -->
        <div class="bg-amber-50/60 rounded-2xl p-3.5 border border-amber-200/80 space-y-3">
          <div class="flex items-center justify-between border-b border-amber-200 pb-2">
            <span class="text-xs font-bold text-amber-900 uppercase">🧾 Parchi ke Saman</span>
            <div class="flex items-center gap-3">
              <button
                type="button"
                id="copyBillBtn"
                class="text-xs text-emerald-700 hover:text-emerald-900 font-semibold"
              >
                📲 Parchi Copy Karein
              </button>
              <button
                type="button"
                id="clearBillBtn"
                class="text-xs text-rose-600 hover:text-rose-800 font-semibold"
              >
                🗑️ Parchi Saaf Karein
              </button>
            </div>
          </div>

          <div id="billItemsContainer" class="divide-y divide-amber-200/60"></div>

          <div id="billEmptyState" class="text-center py-5 text-slate-400 text-xs">
            Parchi abhi khali hai. Upar se saman chunein aur "+ Parchi" dabayein.
          </div>

          <!-- Grand Total Bar -->
          <div class="pt-2 border-t-2 border-amber-300 flex items-center justify-between">
            <span class="text-sm font-bold text-slate-800">Kul Bill (Grand Total):</span>
            <span id="billGrandTotal" class="text-2xl font-extrabold text-emerald-700">₹0</span>
          </div>
        </div>

        <!-- Option A: Record Cash Sale -->
        <div class="bg-emerald-50 border border-emerald-200 rounded-2xl p-3.5 flex flex-col sm:flex-row sm:items-center justify-between gap-2.5">
          <div>
            <div class="text-xs font-bold text-emerald-900">💵 1. Agar Grahak ne Nagad (Cash/UPI) paise diye:</div>
            <div class="text-[11px] text-emerald-700">Yeh bill "Aaj ki Bikri" me jud jayega aur parchi saaf ho jayegi</div>
          </div>
          <button
            type="button"
            id="recordCashSaleBtn"
            class="bg-emerald-600 hover:bg-emerald-700 text-white font-semibold px-4 py-2 rounded-xl text-xs sm:text-sm transition shrink-0"
          >
            💵 Nagad Bikri Save Karein
          </button>
        </div>

        <!-- Option B: Send Full Parchi to Udhari Khata -->
        <div class="bg-rose-50/70 border border-rose-200 rounded-2xl p-3.5 space-y-2">
          <div class="text-xs font-bold text-rose-900">
            📒 2. Agar Grahak ne yeh bill Udhari me liya hai:
          </div>
          <div class="flex flex-col sm:flex-row gap-2">
            <input
              id="billCustomerInput"
              type="text"
              placeholder="Grahak ka naam (jaise: Aditya Kumar)"
              class="flex-1 px-3.5 py-2 rounded-xl border border-rose-200 bg-white focus:outline-none focus:ring-2 focus:ring-rose-500 text-sm"
            />
            <button
              type="button"
              id="sendBillToUdhariBtn"
              class="bg-rose-600 hover:bg-rose-700 text-white font-semibold px-4 py-2 rounded-xl text-xs sm:text-sm transition"
            >
              📒 Pura Bill Udhari me Jodein
            </button>
          </div>
        </div>
      </section>

      <!-- Aaj ka Nagad Galla / Bikri Summary Card -->
      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-emerald-100 space-y-3">
        <div class="flex items-center justify-between">
          <div>
            <h3 class="text-sm font-bold text-slate-800">💵 Aaj ki Nagad Bikri (Galla Hisab)</h3>
            <p class="text-xs text-slate-500">Din bhar me jitni nagad parchi kati unka hisab</p>
          </div>
          <button
            type="button"
            id="resetSalesBtn"
            class="text-xs bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold px-2.5 py-1.5 rounded-lg transition"
          >
            🔄 Naya Din (Galla ₹0 Karein)
          </button>
        </div>
        <div id="salesLogContainer" class="space-y-1.5 text-xs text-slate-700"></div>
      </section>
    </div>

    <!-- ========================================== -->
    <!-- VIEW 4: CLEAR UDHARI KHATA + WHATSAPP      -->
    <!-- ========================================== -->
    <div id="udhariView" class="hidden space-y-4">
      <section class="bg-white rounded-2xl p-4 shadow-sm border-2 border-amber-200 space-y-2">
        <div class="text-xs sm:text-sm font-bold text-amber-900 flex items-center gap-1.5">
          <span>💡 Udhari Khata Kaise Chalayein? (3 Aasan Button):</span>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-2 text-xs">
          <div class="bg-rose-50 border border-rose-200 p-2.5 rounded-xl text-rose-900">
            <strong>1. 🔴 और उधारी (+ Diye):</strong> Jab grahak aur saman udhar le, to rakam likhkar लाल बटन dabayein.
          </div>
          <div class="bg-emerald-50 border border-emerald-200 p-2.5 rounded-xl text-emerald-900">
            <strong>2. 🟢 पैसे मिले (- Jama):</strong> Jab grahak kuch paise jama kare, to rakam likhkar हरा बटन dabayein.
          </div>
          <div class="bg-green-50 border border-green-300 p-2.5 rounded-xl text-green-950">
            <strong>3. 💬 WhatsApp Bhejein:</strong> Grahak ka mobile number jodkar seedha WhatsApp par baki hisaab bhejein!
          </div>
        </div>
      </section>

      <section id="reminderPreviewBox" class="hidden bg-emerald-50 rounded-2xl p-4 shadow-sm border-2 border-emerald-400 space-y-2.5">
        <div class="flex items-center justify-between">
          <span class="text-xs font-bold text-emerald-900 uppercase tracking-wide">
            📲 WhatsApp / SMS Reminder Taiyaar (Copy Ho Gaya!)
          </span>
          <button
            type="button"
            id="closeReminderBoxBtn"
            class="text-xs text-emerald-800 hover:text-emerald-950 font-semibold px-2 py-0.5 rounded"
          >
            Band Karein
          </button>
        </div>
        <p id="reminderTextOutput" class="text-sm text-slate-800 bg-white p-3 rounded-xl border border-emerald-200"></p>
      </section>

      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-rose-100">
        <h2 class="text-base font-semibold text-slate-800 mb-3 flex items-center gap-2">
          <span>📝 Naye Grahak ki Udhari Likhein</span>
        </h2>
        <form id="addUdhariForm" class="grid grid-cols-1 sm:grid-cols-12 gap-2.5">
          <input
            id="customerNameInput"
            type="text"
            placeholder="Grahak ka naam (jaise: Aditya Kumar)"
            required
            class="sm:col-span-3 px-3 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500 text-sm"
          />
          <input
            id="customerPhoneInput"
            type="tel"
            placeholder="Mobile No. (10 ank)"
            class="sm:col-span-3 px-3 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500 text-sm"
          />
          <input
            id="udhariAmountInput"
            type="number"
            min="1"
            step="any"
            placeholder="Baki ₹ (jaise: 450)"
            required
            class="sm:col-span-2 px-3 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500 text-sm"
          />
          <input
            id="udhariNoteInput"
            type="text"
            placeholder="Saman (jaise: Atta)"
            class="sm:col-span-2 px-3 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500 text-sm"
          />
          <button
            type="submit"
            id="saveUdhariBtn"
            class="sm:col-span-2 bg-rose-600 hover:bg-rose-700 active:bg-rose-800 text-white font-semibold py-2.5 px-3 rounded-xl text-sm transition"
          >
            Khata Jodein
          </button>
        </form>
      </section>

      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-rose-100 space-y-4">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2">
          <h2 class="text-base font-semibold text-slate-800">📒 Grahak Udhari Khata List</h2>
          <div class="relative flex-1 sm:max-w-xs">
            <input
              id="udhariSearchInput"
              type="text"
              placeholder="🔍 Grahak ka naam ya mobile khojein..."
              class="w-full pl-3.5 pr-8 py-2 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-rose-500 text-sm bg-slate-50"
            />
          </div>
        </div>

        <div id="udhariListContainer" class="space-y-3"></div>

        <div id="udhariEmptyState" class="hidden text-center py-8 text-slate-400 text-sm">
          Koi udhari baki nahi hai!
        </div>
      </section>
    </div>

    <!-- ========================================== -->
    <!-- VIEW 5: MANDI LIST + DUKAN KHARCHA         -->
    <!-- ========================================== -->
    <div id="mandiView" class="hidden space-y-4">
      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-indigo-100 space-y-3">
        <div>
          <h2 class="text-base font-semibold text-slate-800 flex items-center gap-2">
            <span>📦 Bazar / Mandi se Mangwane Wala Saman</span>
          </h2>
          <p class="text-xs text-slate-500 mt-0.5">
            Jo saman dukan me khatam hone wala hai use yahan likhein
          </p>
        </div>

        <form id="addMandiForm" class="grid grid-cols-1 sm:grid-cols-12 gap-2.5">
          <input
            id="mandiItemInput"
            type="text"
            placeholder="Saman ka naam (jaise: Chawal, Maggi, Sabun)"
            required
            class="sm:col-span-6 px-3.5 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm"
          />
          <input
            id="mandiQtyInput"
            type="text"
            placeholder="Kitna mangwana hai? (jaise: 2 Bori, 3 Peti)"
            required
            class="sm:col-span-4 px-3.5 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm"
          />
          <button
            type="submit"
            id="saveMandiBtn"
            class="sm:col-span-2 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-2.5 px-3 rounded-xl text-sm transition"
          >
            List me Jodein
          </button>
        </form>
      </section>

      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-indigo-100 space-y-4">
        <div class="flex flex-wrap items-center justify-between gap-2">
          <h3 class="text-base font-semibold text-slate-800">📝 Mandi Order Checklist</h3>
          <button
            type="button"
            id="copyMandiBtn"
            class="text-xs bg-indigo-50 hover:bg-indigo-100 text-indigo-800 border border-indigo-200 font-semibold px-3 py-1.5 rounded-xl transition"
          >
            📲 Mahajan ke liye List Copy Karein
          </button>
        </div>

        <div id="mandiListContainer" class="divide-y divide-slate-100"></div>

        <div id="mandiEmptyState" class="hidden text-center py-8 text-slate-400 text-sm">
          Abhi koi saman mangwane ke liye baki nahi hai!
        </div>
      </section>

      <!-- Dukan Kharcha / Mahajan Payment Note -->
      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-amber-200 space-y-4">
        <div class="flex flex-wrap items-center justify-between gap-2">
          <div>
            <h3 class="text-base font-semibold text-slate-800">💸 Dukan Kharcha / Mahajan Payment</h3>
            <p class="text-xs text-slate-500">Mahajan ko diye paise, bhada ya dukan ke kharche likhein</p>
          </div>
          <div class="bg-amber-50 border border-amber-200 px-3 py-1.5 rounded-xl text-right">
            <div class="text-[11px] text-amber-800 font-medium">Kul Kharcha</div>
            <div id="totalKharchaBadge" class="text-base font-bold text-amber-900">₹0</div>
          </div>
        </div>

        <form id="addKharchaForm" class="grid grid-cols-1 sm:grid-cols-12 gap-2.5">
          <input
            id="kharchaTitleInput"
            type="text"
            placeholder="Kiska kharcha? (jaise: Mahajan Tel Payment, Auto Bhada)"
            required
            class="sm:col-span-7 px-3.5 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm"
          />
          <input
            id="kharchaAmountInput"
            type="number"
            min="1"
            step="any"
            placeholder="Rakam (₹)"
            required
            class="sm:col-span-3 px-3.5 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm"
          />
          <button
            type="submit"
            id="saveKharchaBtn"
            class="sm:col-span-2 bg-amber-600 hover:bg-amber-700 text-white font-semibold py-2.5 px-3 rounded-xl text-sm transition"
          >
            Kharcha Jodein
          </button>
        </form>

        <div id="kharchaListContainer" class="divide-y divide-slate-100"></div>
      </section>
    </div>

    <!-- ========================================== -->
    <!-- VIEW 6: PROFIT, UDHARI, LOSS -> GOOGLE SHEET-->
    <!-- ========================================== -->
    <div id="sheetView" class="hidden space-y-4">
      <!-- Clear Box Explaining Where the Google Sheet is Saved! -->
      <section class="bg-emerald-700 text-white rounded-2xl p-4 md:p-5 shadow-md space-y-3">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
          <div class="space-y-1">
            <span class="inline-block text-[11px] font-bold uppercase bg-emerald-900/80 text-emerald-200 px-2.5 py-0.5 rounded-full">
              ✅ आपकी Google Sheet तैयार है
            </span>
            <h2 class="text-lg font-bold">📂 आपकी Google Sheet कहाँ सेव हो रही है?</h2>
            <p class="text-xs text-emerald-100 leading-relaxed">
              1. <strong>आपके Google Drive में:</strong> हमने आपके Google Account में <strong>"Dukan Sathi - Profit, Udhari &amp; Loss Hisab"</strong> नाम की असली Google Sheet बना दी है (जिसमें 3 टैब हैं: Daily Hisab, Udhari Khata, Rate Calculator)।<br>
              2. <strong>इस ऐप के अंदर:</strong> नीचे दिख रही टेबल आपके फोन/ब्राउज़र में तुरंत सेव रहती है।
            </p>
          </div>
          <a
            href="https://docs.google.com/spreadsheets/d/1TUnY1im5H2oKU6B8fboZrqevnXuHyAZFWYbe9T8R5KU/edit"
            target="_blank"
            rel="noopener noreferrer"
            class="bg-white hover:bg-emerald-50 text-emerald-900 font-extrabold text-xs sm:text-sm px-4 py-3 rounded-xl shadow transition text-center shrink-0"
          >
            📂 अपनी Google Sheet अभी खोलें ↗️
          </a>
        </div>
      </section>

      <!-- 1. Live Profit / Udhari / Loss Summary Dashboard -->
      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border-2 border-emerald-200 space-y-4">
        <div class="flex flex-wrap items-center justify-between gap-2">
          <div>
            <h2 class="text-base font-bold text-slate-900 flex items-center gap-2">
              <span>📊 Dukan Profit, Udhari aur Loss Summary</span>
            </h2>
            <p class="text-xs text-slate-500">
              Aapki Bikri, Udhari aur Kharcha/Loss ka pura hisab
            </p>
          </div>
          <button
            type="button"
            id="saveCurrentToSheetTableBtn"
            class="text-xs bg-emerald-600 hover:bg-emerald-700 text-white font-semibold px-3.5 py-2 rounded-xl transition"
          >
            ➕ Aaj ka Hisab Sheet Table me Jodein
          </button>
        </div>

        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2.5">
          <div class="bg-emerald-50 border border-emerald-200 p-3 rounded-xl">
            <div class="text-[11px] font-semibold text-emerald-800">💵 कुल बिक्री (Sales)</div>
            <div id="sheetSalesStat" class="text-lg font-extrabold text-emerald-700 mt-0.5">₹0</div>
          </div>
          <div class="bg-amber-50 border border-amber-200 p-3 rounded-xl">
            <div class="text-[11px] font-semibold text-amber-900">💸 खर्च / नुकसान (Loss)</div>
            <div id="sheetLossStat" class="text-lg font-extrabold text-amber-800 mt-0.5">₹0</div>
          </div>
          <div class="bg-rose-50 border border-rose-200 p-3 rounded-xl">
            <div class="text-[11px] font-semibold text-rose-800">📒 कुल उधारी (Udhari)</div>
            <div id="sheetUdhariStat" class="text-lg font-extrabold text-rose-700 mt-0.5">₹0</div>
          </div>
          <div id="sheetNetCard" class="bg-indigo-50 border border-indigo-200 p-3 rounded-xl">
            <div id="sheetNetLabel" class="text-[11px] font-semibold text-indigo-900">📈 शुद्ध मुनाफा (Profit)</div>
            <div id="sheetNetStat" class="text-lg font-extrabold text-indigo-700 mt-0.5">+₹0</div>
          </div>
        </div>

        <!-- Custom Manual Entry to Sheet Log -->
        <form id="addSheetRowForm" class="bg-slate-50 p-3.5 rounded-2xl border border-slate-200 space-y-2.5">
          <div class="text-xs font-bold text-slate-700">
            ✍️ Naya Din / Custom Profit-Udhari-Loss Entry Sheet me Jodein:
          </div>
          <div class="grid grid-cols-1 sm:grid-cols-12 gap-2">
            <input
              id="sheetDateInput"
              type="text"
              placeholder="Tareekh (jaise: 27 Oct)"
              required
              class="sm:col-span-2 px-3 py-2 rounded-xl border border-slate-200 bg-white text-xs sm:text-sm"
            />
            <input
              id="sheetProfitInput"
              type="number"
              min="0"
              step="any"
              placeholder="Profit / Bikri ₹"
              required
              class="sm:col-span-3 px-3 py-2 rounded-xl border border-slate-200 bg-white text-xs sm:text-sm"
            />
            <input
              id="sheetUdhariInput"
              type="number"
              min="0"
              step="any"
              placeholder="Udhari ₹"
              required
              class="sm:col-span-2 px-3 py-2 rounded-xl border border-slate-200 bg-white text-xs sm:text-sm"
            />
            <input
              id="sheetLossInput"
              type="number"
              min="0"
              step="any"
              placeholder="Loss / Kharcha ₹"
              required
              class="sm:col-span-3 px-3 py-2 rounded-xl border border-slate-200 bg-white text-xs sm:text-sm"
            />
            <button
              type="submit"
              id="addSheetRowBtn"
              class="sm:col-span-2 bg-emerald-600 hover:bg-emerald-700 text-white font-semibold py-2 px-3 rounded-xl text-xs sm:text-sm transition"
            >
              + Sheet Row
            </button>
          </div>
        </form>
      </section>

      <!-- 2. Google Sheet Export & Direct Sync Controls -->
      <section class="bg-white rounded-2xl p-4 md:p-5 shadow-sm border border-emerald-200 space-y-4">
        <div>
          <h3 class="text-base font-bold text-slate-800">🟢 अपनी Google Sheet में नया डेटा कैसे डालें?</h3>
          <p class="text-xs text-slate-500">नीचे बटन दबाकर कॉपी करें और अपनी सेव Google Sheet में पेस्ट कर दें, या CSV फाइल डाउनलोड करें</p>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-3 gap-2.5">
          <button
            type="button"
            id="copyForSheetBtn"
            class="bg-emerald-600 hover:bg-emerald-700 text-white font-semibold py-2.5 px-3 rounded-xl text-xs sm:text-sm transition flex items-center justify-center gap-1.5"
          >
            <span>📋 1. Copy for Google Sheet</span>
          </button>
          <button
            type="button"
            id="downloadCsvBtn"
            class="bg-teal-600 hover:bg-teal-700 text-white font-semibold py-2.5 px-3 rounded-xl text-xs sm:text-sm transition flex items-center justify-center gap-1.5"
          >
            <span>📥 2. Download Sheet (.csv)</span>
          </button>
          <a
            href="https://docs.google.com/spreadsheets/d/1TUnY1im5H2oKU6B8fboZrqevnXuHyAZFWYbe9T8R5KU/edit"
            target="_blank"
            rel="noopener noreferrer"
            id="openNewSheetLink"
            class="bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-300 font-semibold py-2.5 px-3 rounded-xl text-xs sm:text-sm transition flex items-center justify-center gap-1.5"
          >
            <span>↗️ Nayi Google Sheet Kholein</span>
          </a>
        </div>

        <!-- Way 3: Direct Google Apps Script Webhook Sync -->
        <div class="bg-emerald-50/70 border border-emerald-200 rounded-2xl p-3.5 space-y-2.5">
          <div class="flex items-center justify-between">
            <span class="text-xs font-bold text-emerald-950">
              🔗 3. Direct Google Sheet Auto-Send (Apps Script URL):
            </span>
            <button
              type="button"
              id="toggleScriptGuideBtn"
              class="text-xs text-emerald-800 underline font-semibold"
            >
              📖 Setup Code Dekhein
            </button>
          </div>

          <div class="flex flex-col sm:flex-row gap-2">
            <input
              id="sheetWebhookInput"
              type="url"
              placeholder="Google Apps Script Web App URL yahan paste karein (https://script.google.com/...)"
              class="flex-1 px-3 py-2 rounded-xl border border-emerald-300 bg-white text-xs sm:text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500"
            />
            <button
              type="button"
              id="sendToGoogleSheetBtn"
              class="bg-emerald-700 hover:bg-emerald-800 text-white font-semibold px-4 py-2 rounded-xl text-xs sm:text-sm transition shrink-0"
            >
              🚀 Seedhe Sheet me Bhejein
            </button>
          </div>

          <!-- Collapsible Apps Script Guide -->
          <div id="scriptGuideBox" class="hidden bg-white p-3 rounded-xl border border-emerald-200 text-xs text-slate-700 space-y-2">
            <div class="font-bold text-slate-900">Apni Google Sheet me Auto-Add kaise chalu karein (1 baar ka kaam):</div>
            <ol class="list-decimal list-inside space-y-1">
              <li>Apni Google Sheet kholein aur upar <strong>Extensions &rarr; Apps Script</strong> par click karein.</li>
              <li>Wahan niche diya gaya chhota code paste karke <strong>Deploy &rarr; New deployment &rarr; Web app (Anyone)</strong> karein.</li>
              <li>Milne wala URL upar box me daal kar <strong>🚀 Seedhe Sheet me Bhejein</strong> dabayein!</li>
            </ol>
            <div class="flex items-center justify-between bg-slate-900 text-emerald-300 p-2.5 rounded-lg font-mono text-[11px] overflow-x-auto">
              <pre id="appsScriptCodeText">function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var d = JSON.parse(e.postData.contents);
  sheet.appendRow([d.date, d.shop, d.profit, d.udhari, d.loss, d.net, d.note]);
  return ContentService.createTextOutput("OK");
}</pre>
            </div>
            <button
              type="button"
              id="copyAppsScriptBtn"
              class="text-xs bg-slate-800 hover:bg-slate-900 text-white font-semibold px-3 py-1.5 rounded-lg"
            >
              📋 Apps Script Code Copy Karein
            </button>
          </div>
        </div>

        <!-- Sheet Table Preview -->
        <div class="space-y-2">
          <div class="text-xs font-bold text-slate-700 uppercase">📋 Google Sheet Rows Preview (Date | Profit/Bikri | Udhari | Loss/Kharcha | Net Bachat):</div>
          <div class="overflow-x-auto border border-slate-200 rounded-xl">
            <table class="w-full text-left text-xs">
              <thead class="bg-slate-100 text-slate-700 font-bold border-b border-slate-200">
                <tr>
                  <th class="p-2.5">Tareekh</th>
                  <th class="p-2.5 text-emerald-700">Profit/Bikri</th>
                  <th class="p-2.5 text-rose-700">Udhari</th>
                  <th class="p-2.5 text-amber-800">Loss/Kharcha</th>
                  <th class="p-2.5 text-indigo-800">Net Bachat</th>
                  <th class="p-2.5 text-right">Action</th>
                </tr>
              </thead>
              <tbody id="sheetRowsContainer" class="divide-y divide-slate-100"></tbody>
            </table>
          </div>
        </div>
      </section>
    </div>

    <!-- Footer with Backup Download & Reset Sample Data Button -->
    <footer class="bg-amber-100/80 border border-amber-200 rounded-2xl p-4 text-xs text-amber-900 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
      <div>
        <strong>💾 Auto-Save + Google Sheet Ready:</strong> Aapka Profit, Udhari aur Loss hisab hamesha surakshit hai.
      </div>
      <div class="flex flex-wrap items-center gap-2 shrink-0">
        <button
          type="button"
          id="downloadBackupBtn"
          class="text-xs bg-emerald-600 hover:bg-emerald-700 text-white font-semibold px-3 py-1.5 rounded-lg transition"
        >
          📥 Khata Backup Download
        </button>
        <button
          type="button"
          id="resetDataBtn"
          class="text-xs bg-white hover:bg-amber-50 text-amber-900 border border-amber-300 font-medium px-3 py-1.5 rounded-lg transition"
        >
          🔄 Sample Data Reset
        </button>
      </div>
    </footer>
  </div>

  <script>
    // 1. DEFAULT SAMPLE DATA (Cheeni updated to Rs 45/kg as requested by user!)
    const CATEGORY_LABELS = {
      anaj: "🌾 अनाज/दाल",
      tel: "🛢️ तेल/मसाले",
      packet: "🍪 पैकेट/चाय",
      sabun: "🧼 साबुन/अन्य"
    };

    const DEFAULT_ITEMS = [
      { id: 3, name: "Cheeni (Sugar)", price: 45, unit: "kg", category: "anaj" },
      { id: 1, name: "Basmati Chawal", price: 90, unit: "kg", category: "anaj" },
      { id: 2, name: "Aashirvaad Atta", price: 45, unit: "kg", category: "anaj" },
      { id: 4, name: "Sarso Tel (Mustard Oil)", price: 165, unit: "ltr", category: "tel" },
      { id: 5, name: "Toor Dal (Arhar)", price: 140, unit: "kg", category: "anaj" },
      { id: 6, name: "Tata Chai Patti (250g)", price: 110, unit: "pkt", category: "packet" },
      { id: 7, name: "Lifebuoy Sabun", price: 36, unit: "pc", category: "sabun" }
    ];

    const DEFAULT_UDHARI = [
      {
        id: 100,
        customer: "Aditya Kumar",
        phone: "9876543210",
        amount: 450,
        note: "Dukan saman",
        history: ["🔴 ₹450 उधारी दिए (Dukan saman)"]
      },
      {
        id: 101,
        customer: "Ramesh Kumar",
        phone: "",
        amount: 340,
        note: "5kg Atta, 1L Tel",
        history: ["🔴 ₹340 उधारी दिए (5kg Atta, 1L Tel)"]
      },
      {
        id: 102,
        customer: "Sunita Devi",
        phone: "",
        amount: 120,
        note: "Cheeni aur Chai Patti",
        history: ["🔴 ₹120 उधारी दिए (Cheeni aur Chai Patti)"]
      },
      {
        id: 103,
        customer: "Mohan Lal",
        phone: "",
        amount: 550,
        note: "Mahine ka rashan baki",
        history: ["🔴 ₹550 उधारी दिए (Mahine ka rashan baki)"]
      }
    ];

    const DEFAULT_MANDI = [
      { id: 501, name: "Sarso Tel (Mustard Oil)", qty: "2 Tin ( पेटी )" },
      { id: 502, name: "Cheeni (Sugar)", qty: "1 Bori (50 kg)" }
    ];

    const DEFAULT_KHARCHA = [
      { id: 701, title: "Mandi Auto Bhada", amount: 150 }
    ];

    const DEFAULT_SHEET_ROWS = [
      { id: 901, date: "26 Oct", profit: 1850, udhari: 450, loss: 200, net: 1650 },
      { id: 902, date: "27 Oct", profit: 2400, udhari: 1460, loss: 150, net: 2250 }
    ];

    const STORAGE_KEY_ITEMS = "dukan_sathi_items_v3";
    const STORAGE_KEY_UDHARI = "dukan_sathi_udhari_v3";
    const STORAGE_KEY_SHOP_NAME = "dukan_sathi_shop_name_v3";
    const STORAGE_KEY_SALES = "dukan_sathi_sales_v3";
    const STORAGE_KEY_MANDI = "dukan_sathi_mandi_v3";
    const STORAGE_KEY_KHARCHA = "dukan_sathi_kharcha_v3";
    const STORAGE_KEY_SHEET_ROWS = "dukan_sathi_sheet_rows_v3";
    const STORAGE_KEY_WEBHOOK = "dukan_sathi_webhook_v3";

    let shopName = "Dukan Sathi";
    let items = [];
    let udhariList = [];
    let billItems = [];
    let salesLog = [];
    let mandiList = [];
    let kharchaList = [];
    let sheetRows = [];
    let sheetWebhookUrl = "";
    let selectedCategory = "all";
    let searchQuery = "";
    let udhariSearchQuery = "";
    let editingItemId = null;
    let toastTimer = null;

    // 2. LOCALSTORAGE LOAD & SAVE
    function guessCategory(name) {
      const lower = String(name || "").toLowerCase();
      if (lower.includes("tel") || lower.includes("masala") || lower.includes("haldi") || lower.includes("namak")) return "tel";
      if (lower.includes("chai") || lower.includes("maggi") || lower.includes("biscuit") || lower.includes("pkt")) return "packet";
      if (lower.includes("sabun") || lower.includes("surf") || lower.includes("shampoo")) return "sabun";
      return "anaj";
    }

    function loadFromStorage() {
      try {
        const savedShop = localStorage.getItem(STORAGE_KEY_SHOP_NAME);
        if (savedShop && savedShop.trim()) {
          shopName = savedShop.trim();
        }
        const savedWebhook = localStorage.getItem(STORAGE_KEY_WEBHOOK);
        if (savedWebhook) {
          sheetWebhookUrl = savedWebhook.trim();
        }
        const savedItems = localStorage.getItem(STORAGE_KEY_ITEMS);
        const savedUdhari = localStorage.getItem(STORAGE_KEY_UDHARI);
        const savedSales = localStorage.getItem(STORAGE_KEY_SALES);
        const savedMandi = localStorage.getItem(STORAGE_KEY_MANDI);
        const savedKharcha = localStorage.getItem(STORAGE_KEY_KHARCHA);
        const savedSheetRows = localStorage.getItem(STORAGE_KEY_SHEET_ROWS);

        items = savedItems ? JSON.parse(savedItems) : structuredClone(DEFAULT_ITEMS);
        udhariList = savedUdhari ? JSON.parse(savedUdhari) : structuredClone(DEFAULT_UDHARI);
        salesLog = savedSales ? JSON.parse(savedSales) : [];
        mandiList = savedMandi ? JSON.parse(savedMandi) : structuredClone(DEFAULT_MANDI);
        kharchaList = savedKharcha ? JSON.parse(savedKharcha) : structuredClone(DEFAULT_KHARCHA);
        sheetRows = savedSheetRows ? JSON.parse(savedSheetRows) : structuredClone(DEFAULT_SHEET_ROWS);

        // Ensure Cheeni is set to 45/kg if it was 44 from earlier default
        items.forEach((item) => {
          if (item.name.toLowerCase().includes("cheeni") && item.price === 44) {
            item.price = 45;
          }
          if (!item.category || !CATEGORY_LABELS[item.category]) {
            item.category = guessCategory(item.name);
          }
        });

        udhariList.forEach((entry) => {
          if (typeof entry.phone !== "string") entry.phone = "";
          if (!Array.isArray(entry.history) || entry.history.length === 0) {
            entry.history = [`🔴 ₹${entry.amount} उधारी दिए (${entry.note || "Dukan saman"})`];
          }
        });
      } catch (err) {
        items = structuredClone(DEFAULT_ITEMS);
        udhariList = structuredClone(DEFAULT_UDHARI);
        salesLog = [];
        mandiList = structuredClone(DEFAULT_MANDI);
        kharchaList = structuredClone(DEFAULT_KHARCHA);
        sheetRows = structuredClone(DEFAULT_SHEET_ROWS);
      }
    }

    function saveToStorage() {
      try {
        localStorage.setItem(STORAGE_KEY_SHOP_NAME, shopName);
        localStorage.setItem(STORAGE_KEY_WEBHOOK, sheetWebhookUrl);
        localStorage.setItem(STORAGE_KEY_ITEMS, JSON.stringify(items));
        localStorage.setItem(STORAGE_KEY_UDHARI, JSON.stringify(udhariList));
        localStorage.setItem(STORAGE_KEY_SALES, JSON.stringify(salesLog));
        localStorage.setItem(STORAGE_KEY_MANDI, JSON.stringify(mandiList));
        localStorage.setItem(STORAGE_KEY_KHARCHA, JSON.stringify(kharchaList));
        localStorage.setItem(STORAGE_KEY_SHEET_ROWS, JSON.stringify(sheetRows));
      } catch (err) {
        // Ignore storage error in restricted iframe
      }
    }

    function copyTextToClipboard(text) {
      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(text).catch(() => fallbackCopy(text));
      } else {
        fallbackCopy(text);
      }
    }

    function fallbackCopy(text) {
      const tempArea = document.createElement("textarea");
      tempArea.value = text;
      tempArea.style.position = "fixed";
      tempArea.style.opacity = "0";
      document.body.appendChild(tempArea);
      tempArea.select();
      try {
        document.execCommand("copy");
      } catch (err) {
        // Ignore
      }
      document.body.removeChild(tempArea);
    }

    // 3. ELEMENTS
    const shopNameHeading = document.getElementById("shopNameHeading");
    const editShopNameBtn = document.getElementById("editShopNameBtn");
    const shopNameForm = document.getElementById("shopNameForm");
    const shopNameInput = document.getElementById("shopNameInput");
    const cancelShopNameBtn = document.getElementById("cancelShopNameBtn");
    const todaySalesCount = document.getElementById("todaySalesCount");
    const headerNetProfitCount = document.getElementById("headerNetProfitCount");

    const tabRateListBtn = document.getElementById("tabRateListBtn");
    const tabCalcBtn = document.getElementById("tabCalcBtn");
    const tabBillBtn = document.getElementById("tabBillBtn");
    const tabUdhariBtn = document.getElementById("tabUdhariBtn");
    const tabMandiBtn = document.getElementById("tabMandiBtn");
    const tabSheetBtn = document.getElementById("tabSheetBtn");

    const rateListView = document.getElementById("rateListView");
    const calcView = document.getElementById("calcView");
    const billView = document.getElementById("billView");
    const udhariView = document.getElementById("udhariView");
    const mandiView = document.getElementById("mandiView");
    const sheetView = document.getElementById("sheetView");

    const editRateBox = document.getElementById("editRateBox");
    const editItemNameLabel = document.getElementById("editItemNameLabel");
    const editRateForm = document.getElementById("editRateForm");
    const editRateInput = document.getElementById("editRateInput");
    const cancelEditRateBtn = document.getElementById("cancelEditRateBtn");

    const addItemForm = document.getElementById("addItemForm");
    const itemNameInput = document.getElementById("itemNameInput");
    const itemPriceInput = document.getElementById("itemPriceInput");
    const itemUnitInput = document.getElementById("itemUnitInput");
    const itemCategoryInput = document.getElementById("itemCategoryInput");
    const searchInput = document.getElementById("searchInput");
    const clearSearchBtn = document.getElementById("clearSearchBtn");
    const itemsListContainer = document.getElementById("itemsListContainer");
    const emptyState = document.getElementById("emptyState");
    const totalItemsCount = document.getElementById("totalItemsCount");

    const catAllBtn = document.getElementById("catAllBtn");
    const catAnajBtn = document.getElementById("catAnajBtn");
    const catTelBtn = document.getElementById("catTelBtn");
    const catPacketBtn = document.getElementById("catPacketBtn");
    const catSabunBtn = document.getElementById("catSabunBtn");

    // Direct Gram Calculator Elements
    const directRateKgInput = document.getElementById("directRateKgInput");
    const directGramsInput = document.getElementById("directGramsInput");
    const directFormulaExplain = document.getElementById("directFormulaExplain");
    const directStepByStepMath = document.getElementById("directStepByStepMath");
    const directExactRupees = document.getElementById("directExactRupees");
    const directRoundedRupees = document.getElementById("directRoundedRupees");
    const reverseRupeeInput = document.getElementById("reverseRupeeInput");
    const reverseGramsOutput = document.getElementById("reverseGramsOutput");

    const direct50gBtn = document.getElementById("direct50gBtn");
    const direct100gBtn = document.getElementById("direct100gBtn");
    const direct200gBtn = document.getElementById("direct200gBtn");
    const direct250gBtn = document.getElementById("direct250gBtn");
    const direct450gBtn = document.getElementById("direct450gBtn");
    const direct500gBtn = document.getElementById("direct500gBtn");
    const direct750gBtn = document.getElementById("direct750gBtn");
    const direct1000gBtn = document.getElementById("direct1000gBtn");

    const calcItemSelect = document.getElementById("calcItemSelect");
    const calcQtyInput = document.getElementById("calcQtyInput");
    const calcUnitMode = document.getElementById("calcUnitMode");
    const calcSummaryText = document.getElementById("calcSummaryText");
    const calcResultPrice = document.getElementById("calcResultPrice");
    const calcCustomerInput = document.getElementById("calcCustomerInput");
    const sendCalcToUdhariBtn = document.getElementById("sendCalcToUdhariBtn");

    const preset100gBtn = document.getElementById("preset100gBtn");
    const preset250gBtn = document.getElementById("preset250gBtn");
    const preset450gBtn = document.getElementById("preset450gBtn");
    const preset500gBtn = document.getElementById("preset500gBtn");
    const preset1kgBtn = document.getElementById("preset1kgBtn");
    const preset2kgBtn = document.getElementById("preset2kgBtn");
    const preset5kgBtn = document.getElementById("preset5kgBtn");

    const addToBillForm = document.getElementById("addToBillForm");
    const billItemSelect = document.getElementById("billItemSelect");
    const billQtyInput = document.getElementById("billQtyInput");
    const billUnitMode = document.getElementById("billUnitMode");
    const billItemsContainer = document.getElementById("billItemsContainer");
    const billEmptyState = document.getElementById("billEmptyState");
    const billGrandTotal = document.getElementById("billGrandTotal");
    const billCountBadge = document.getElementById("billCountBadge");
    const copyBillBtn = document.getElementById("copyBillBtn");
    const clearBillBtn = document.getElementById("clearBillBtn");
    const recordCashSaleBtn = document.getElementById("recordCashSaleBtn");
    const billCustomerInput = document.getElementById("billCustomerInput");
    const sendBillToUdhariBtn = document.getElementById("sendBillToUdhariBtn");
    const salesLogContainer = document.getElementById("salesLogContainer");
    const resetSalesBtn = document.getElementById("resetSalesBtn");

    const reminderPreviewBox = document.getElementById("reminderPreviewBox");
    const reminderTextOutput = document.getElementById("reminderTextOutput");
    const closeReminderBoxBtn = document.getElementById("closeReminderBoxBtn");

    const addUdhariForm = document.getElementById("addUdhariForm");
    const customerNameInput = document.getElementById("customerNameInput");
    const customerPhoneInput = document.getElementById("customerPhoneInput");
    const udhariAmountInput = document.getElementById("udhariAmountInput");
    const udhariNoteInput = document.getElementById("udhariNoteInput");
    const udhariSearchInput = document.getElementById("udhariSearchInput");
    const udhariListContainer = document.getElementById("udhariListContainer");
    const udhariEmptyState = document.getElementById("udhariEmptyState");
    const totalUdhariCount = document.getElementById("totalUdhariCount");
    const udhariCustomerBadge = document.getElementById("udhariCustomerBadge");

    const mandiCountBadge = document.getElementById("mandiCountBadge");
    const addMandiForm = document.getElementById("addMandiForm");
    const mandiItemInput = document.getElementById("mandiItemInput");
    const mandiQtyInput = document.getElementById("mandiQtyInput");
    const copyMandiBtn = document.getElementById("copyMandiBtn");
    const mandiListContainer = document.getElementById("mandiListContainer");
    const mandiEmptyState = document.getElementById("mandiEmptyState");

    const totalKharchaBadge = document.getElementById("totalKharchaBadge");
    const addKharchaForm = document.getElementById("addKharchaForm");
    const kharchaTitleInput = document.getElementById("kharchaTitleInput");
    const kharchaAmountInput = document.getElementById("kharchaAmountInput");
    const kharchaListContainer = document.getElementById("kharchaListContainer");

    const sheetSalesStat = document.getElementById("sheetSalesStat");
    const sheetLossStat = document.getElementById("sheetLossStat");
    const sheetUdhariStat = document.getElementById("sheetUdhariStat");
    const sheetNetCard = document.getElementById("sheetNetCard");
    const sheetNetLabel = document.getElementById("sheetNetLabel");
    const sheetNetStat = document.getElementById("sheetNetStat");
    const saveCurrentToSheetTableBtn = document.getElementById("saveCurrentToSheetTableBtn");
    const addSheetRowForm = document.getElementById("addSheetRowForm");
    const sheetDateInput = document.getElementById("sheetDateInput");
    const sheetProfitInput = document.getElementById("sheetProfitInput");
    const sheetUdhariInput = document.getElementById("sheetUdhariInput");
    const sheetLossInput = document.getElementById("sheetLossInput");
    const copyForSheetBtn = document.getElementById("copyForSheetBtn");
    const downloadCsvBtn = document.getElementById("downloadCsvBtn");
    const toggleScriptGuideBtn = document.getElementById("toggleScriptGuideBtn");
    const scriptGuideBox = document.getElementById("scriptGuideBox");
    const appsScriptCodeText = document.getElementById("appsScriptCodeText");
    const copyAppsScriptBtn = document.getElementById("copyAppsScriptBtn");
    const sheetWebhookInput = document.getElementById("sheetWebhookInput");
    const sendToGoogleSheetBtn = document.getElementById("sendToGoogleSheetBtn");
    const sheetRowsContainer = document.getElementById("sheetRowsContainer");

    const downloadBackupBtn = document.getElementById("downloadBackupBtn");
    const resetDataBtn = document.getElementById("resetDataBtn");

    const toastMessage = document.getElementById("toastMessage");

    function showToast(text) {
      toastMessage.textContent = text;
      toastMessage.classList.remove("hidden");
      if (toastTimer) clearTimeout(toastTimer);
      toastTimer = setTimeout(() => {
        toastMessage.classList.add("hidden");
      }, 3200);
    }

    function escapeHtml(str) {
      return String(str)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;");
    }

    function cleanPhone(raw) {
      return String(raw || "").replace(/\D/g, "").slice(-10);
    }

    // Custom Shop Name Logic
    function renderShopName() {
      shopNameHeading.textContent = `🏪 ${shopName}`;
    }

    editShopNameBtn.addEventListener("click", () => {
      shopNameInput.value = shopName;
      shopNameForm.classList.remove("hidden");
      shopNameInput.focus();
    });

    cancelShopNameBtn.addEventListener("click", () => {
      shopNameForm.classList.add("hidden");
    });

    shopNameForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const val = shopNameInput.value.trim();
      if (!val) return;
      shopName = val;
      saveToStorage();
      renderShopName();
      renderUdhari();
      shopNameForm.classList.add("hidden");
      showToast(`🏪 Dukan ka naam "${shopName}" save ho gaya!`);
    });

    // 4. TAB SWITCHING
    function switchTab(tabName) {
      const baseClass = "py-2.5 px-2 rounded-xl font-semibold text-xs sm:text-sm transition flex items-center justify-center gap-1 ";
      const activeClass = baseClass + "bg-white text-amber-700 shadow-sm";
      const inactiveClass = baseClass + "text-amber-900 hover:bg-white/60";

      rateListView.classList.toggle("hidden", tabName !== "rate");
      calcView.classList.toggle("hidden", tabName !== "calc");
      billView.classList.toggle("hidden", tabName !== "bill");
      udhariView.classList.toggle("hidden", tabName !== "udhari");
      mandiView.classList.toggle("hidden", tabName !== "mandi");
      sheetView.classList.toggle("hidden", tabName !== "sheet");

      tabRateListBtn.className = tabName === "rate" ? activeClass : inactiveClass;
      tabCalcBtn.className = tabName === "calc" ? activeClass : inactiveClass;
      tabBillBtn.className = tabName === "bill" ? activeClass : inactiveClass;
      tabUdhariBtn.className = tabName === "udhari" ? activeClass : inactiveClass;
      tabMandiBtn.className = tabName === "mandi" ? activeClass : inactiveClass;
      tabSheetBtn.className = tabName === "sheet"
        ? baseClass + "bg-white text-emerald-800 shadow-sm"
        : inactiveClass;
    }

    tabRateListBtn.addEventListener("click", () => switchTab("rate"));
    tabCalcBtn.addEventListener("click", () => switchTab("calc"));
    tabBillBtn.addEventListener("click", () => switchTab("bill"));
    tabUdhariBtn.addEventListener("click", () => switchTab("udhari"));
    tabMandiBtn.addEventListener("click", () => switchTab("mandi"));
    tabSheetBtn.addEventListener("click", () => switchTab("sheet"));

    // Category Filter Selection
    function setCategoryFilter(cat) {
      selectedCategory = cat;
      const activePill = "text-xs bg-amber-600 text-white font-semibold px-3 py-1.5 rounded-full transition";
      const inactivePill = "text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-3 py-1.5 rounded-full transition";

      catAllBtn.className = cat === "all" ? activePill : inactivePill;
      catAnajBtn.className = cat === "anaj" ? activePill : inactivePill;
      catTelBtn.className = cat === "tel" ? activePill : inactivePill;
      catPacketBtn.className = cat === "packet" ? activePill : inactivePill;
      catSabunBtn.className = cat === "sabun" ? activePill : inactivePill;

      renderItems();
    }

    catAllBtn.addEventListener("click", () => setCategoryFilter("all"));
    catAnajBtn.addEventListener("click", () => setCategoryFilter("anaj"));
    catTelBtn.addEventListener("click", () => setCategoryFilter("tel"));
    catPacketBtn.addEventListener("click", () => setCategoryFilter("packet"));
    catSabunBtn.addEventListener("click", () => setCategoryFilter("sabun"));

    // 5. RENDER SAMAN RATE LIST (With 1-Click '🧮 ग्राम हिसाब' button on each item!)
    function renderItems() {
      totalItemsCount.textContent = items.length;
      populateCalculatorSelect();
      populateBillSelect();

      const filtered = items.filter((item) => {
        const matchesSearch = item.name.toLowerCase().includes(searchQuery.toLowerCase().trim());
        const matchesCat = selectedCategory === "all" || item.category === selectedCategory;
        return matchesSearch && matchesCat;
      });

      itemsListContainer.innerHTML = "";

      if (filtered.length === 0) {
        emptyState.classList.remove("hidden");
        return;
      }

      emptyState.classList.add("hidden");

      filtered.forEach((item) => {
        const row = document.createElement("div");
        row.className = "py-3 flex flex-col sm:flex-row sm:items-center justify-between gap-2.5 hover:bg-amber-50/50 px-2 rounded-xl transition";

        const catLabel = CATEGORY_LABELS[item.category] || "🌾 अनाज/दाल";
        const infoDiv = document.createElement("div");
        infoDiv.innerHTML = `
          <div class="flex items-center gap-2">
            <span class="font-medium text-slate-800 text-sm md:text-base">${escapeHtml(item.name)}</span>
            <span class="text-[11px] bg-slate-100 text-slate-600 px-2 py-0.5 rounded-full">${escapeHtml(catLabel)}</span>
          </div>
          <div class="text-xs text-slate-400">Unit: ${escapeHtml(item.unit)} (450g = ₹${Math.round((item.price / 1000) * 450 * 100) / 100})</div>
        `;

        const rightDiv = document.createElement("div");
        rightDiv.className = "flex flex-wrap items-center gap-1.5";

        const priceBadge = document.createElement("span");
        priceBadge.className = "bg-emerald-50 text-emerald-700 border border-emerald-200 font-bold px-3 py-1 rounded-lg text-sm md:text-base";
        priceBadge.textContent = `₹${item.price} / ${item.unit}`;

        const gramHisabBtn = document.createElement("button");
        gramHisabBtn.type = "button";
        gramHisabBtn.className = "text-xs bg-emerald-600 hover:bg-emerald-700 text-white font-semibold px-2.5 py-1.5 rounded-lg transition";
        gramHisabBtn.textContent = "🧮 ग्राम हिसाब";
        gramHisabBtn.addEventListener("click", () => {
          directRateKgInput.value = String(item.price);
          calcItemSelect.value = String(item.id);
          updateDirectGramCalc();
          updateCalculation();
          switchTab("calc");
          showToast(`🧮 "${item.name}" (₹${item.price}/${item.unit}) का ग्राम हिसाब खुल गया!`);
        });

        const mandiQuickBtn = document.createElement("button");
        mandiQuickBtn.type = "button";
        mandiQuickBtn.className = "text-xs bg-indigo-50 hover:bg-indigo-100 text-indigo-800 border border-indigo-200 font-medium px-2.5 py-1.5 rounded-lg transition";
        mandiQuickBtn.textContent = "📦 +Mandi";
        mandiQuickBtn.addEventListener("click", () => quickAddToMandi(item.name));

        const editBtn = document.createElement("button");
        editBtn.type = "button";
        editBtn.className = "text-xs bg-amber-50 hover:bg-amber-100 text-amber-900 border border-amber-200 font-medium px-2.5 py-1.5 rounded-lg transition";
        editBtn.textContent = "✏️ Rate Badlein";
        editBtn.addEventListener("click", () => openEditRateBox(item));

        const deleteBtn = document.createElement("button");
        deleteBtn.type = "button";
        deleteBtn.className = "text-xs text-rose-500 hover:bg-rose-50 px-2.5 py-1.5 rounded-lg border border-transparent hover:border-rose-200 transition";
        deleteBtn.textContent = "Hatayein";
        deleteBtn.addEventListener("click", () => deleteItem(item.id, item.name));

        rightDiv.appendChild(priceBadge);
        rightDiv.appendChild(gramHisabBtn);
        rightDiv.appendChild(mandiQuickBtn);
        rightDiv.appendChild(editBtn);
        rightDiv.appendChild(deleteBtn);

        row.appendChild(infoDiv);
        row.appendChild(rightDiv);
        itemsListContainer.appendChild(row);
      });
    }

    function openEditRateBox(item) {
      editingItemId = item.id;
      editItemNameLabel.textContent = `${item.name} (Abhi: ₹${item.price}/${item.unit})`;
      editRateInput.value = String(item.price);
      editRateBox.classList.remove("hidden");
      editRateInput.focus();
    }

    cancelEditRateBtn.addEventListener("click", () => {
      editingItemId = null;
      editRateBox.classList.add("hidden");
    });

    editRateForm.addEventListener("submit", (e) => {
      e.preventDefault();
      if (editingItemId === null) return;
      const newPrice = parseFloat(editRateInput.value);
      if (isNaN(newPrice) || newPrice <= 0) return;

      const target = items.find((i) => i.id === editingItemId);
      if (target) {
        const oldPrice = target.price;
        target.price = newPrice;
        saveToStorage();
        renderItems();
        showToast(`✏️ "${target.name}" ka rate ₹${oldPrice} se badalkar ₹${newPrice}/${target.unit} ho gaya!`);
      }
      editingItemId = null;
      editRateBox.classList.add("hidden");
    });

    // 6. DIRECT GRAM CALCULATOR + ITEM CALCULATOR LOGIC
    function updateDirectGramCalc() {
      const rateKg = parseFloat(directRateKgInput.value) || 0;
      const grams = parseFloat(directGramsInput.value) || 0;
      const exactPrice = Math.round(((rateKg / 1000) * grams) * 100) / 100;
      const roundedPrice = Math.round(exactPrice);

      directFormulaExplain.textContent = `₹${rateKg}/किलो के भाव से ${grams} ग्राम का दाम:`;
      directStepByStepMath.textContent = `गणित: (₹${rateKg} ÷ 1000 ग्राम) × ${grams} ग्राम = ₹${exactPrice}`;
      directExactRupees.textContent = `₹${exactPrice}`;
      directRoundedRupees.textContent = `ग्राहक से लें (पूरा रुपया): ₹${roundedPrice}`;

      updateReverseRupeeCalc();
    }

    function updateReverseRupeeCalc() {
      const rateKg = parseFloat(directRateKgInput.value) || 0;
      const rupees = parseFloat(reverseRupeeInput.value) || 0;
      if (rateKg <= 0 || rupees <= 0) {
        reverseGramsOutput.textContent = "👉 ₹0 में 0 ग्राम";
        return;
      }
      const gramsToGive = Math.round((rupees / rateKg) * 1000);
      reverseGramsOutput.textContent = `👉 ₹${rupees} में ${gramsToGive} ग्राम तौलें`;
    }

    directRateKgInput.addEventListener("input", updateDirectGramCalc);
    directGramsInput.addEventListener("input", updateDirectGramCalc);
    reverseRupeeInput.addEventListener("input", updateReverseRupeeCalc);

    function setDirectGrams(g) {
      directGramsInput.value = String(g);
      updateDirectGramCalc();
    }

    direct50gBtn.addEventListener("click", () => setDirectGrams(50));
    direct100gBtn.addEventListener("click", () => setDirectGrams(100));
    direct200gBtn.addEventListener("click", () => setDirectGrams(200));
    direct250gBtn.addEventListener("click", () => setDirectGrams(250));
    direct450gBtn.addEventListener("click", () => setDirectGrams(450));
    direct500gBtn.addEventListener("click", () => setDirectGrams(500));
    direct750gBtn.addEventListener("click", () => setDirectGrams(750));
    direct1000gBtn.addEventListener("click", () => setDirectGrams(1000));

    function populateCalculatorSelect() {
      const currentVal = calcItemSelect.value;
      calcItemSelect.innerHTML = "";

      if (items.length === 0) {
        const opt = document.createElement("option");
        opt.value = "";
        opt.textContent = "Pehle Rate List me saman jodein";
        calcItemSelect.appendChild(opt);
        updateCalculation();
        return;
      }

      items.forEach((item) => {
        const opt = document.createElement("option");
        opt.value = String(item.id);
        opt.textContent = `${item.name} (₹${item.price}/${item.unit})`;
        calcItemSelect.appendChild(opt);
      });

      if (currentVal && items.some((i) => String(i.id) === currentVal)) {
        calcItemSelect.value = currentVal;
      }
      updateCalculation();
    }

    function calculateItemPrice(selectedItem, qty, mode) {
      if (!selectedItem || qty <= 0) {
        return { item: selectedItem, total: 0, label: "0" };
      }
      let total = 0;
      let label = "";
      if (mode === "gram") {
        total = (selectedItem.price / 1000) * qty;
        const unitLabel = selectedItem.unit === "ltr" ? "ml" : "gram";
        label = `${qty}${unitLabel} ${selectedItem.name}`;
      } else {
        total = selectedItem.price * qty;
        label = `${qty}${selectedItem.unit} ${selectedItem.name}`;
      }
      const roundedTotal = Math.round(total * 100) / 100;
      return { item: selectedItem, total: roundedTotal, label: label };
    }

    function getCalculatedDetails() {
      const selectedId = Number(calcItemSelect.value);
      const selectedItem = items.find((i) => i.id === selectedId) || items[0];
      const qty = parseFloat(calcQtyInput.value) || 0;
      const mode = calcUnitMode.value;
      return calculateItemPrice(selectedItem, qty, mode);
    }

    function updateCalculation() {
      const details = getCalculatedDetails();
      if (!details.item) {
        calcSummaryText.textContent = "Koi saman nahi chuna gaya";
        calcResultPrice.textContent = "₹0";
        return;
      }
      calcSummaryText.textContent = `${details.label} (@ ₹${details.item.price}/${details.item.unit})`;
      calcResultPrice.textContent = `₹${details.total}`;
    }

    calcItemSelect.addEventListener("change", () => {
      const selectedId = Number(calcItemSelect.value);
      const selectedItem = items.find((i) => i.id === selectedId);
      if (selectedItem) {
        directRateKgInput.value = String(selectedItem.price);
        updateDirectGramCalc();
      }
      updateCalculation();
    });
    calcQtyInput.addEventListener("input", updateCalculation);
    calcUnitMode.addEventListener("change", updateCalculation);

    function applyPreset(qty, mode) {
      calcQtyInput.value = String(qty);
      calcUnitMode.value = mode;
      if (mode === "gram") {
        directGramsInput.value = String(qty);
        updateDirectGramCalc();
      }
      updateCalculation();
    }

    preset100gBtn.addEventListener("click", () => applyPreset(100, "gram"));
    preset250gBtn.addEventListener("click", () => applyPreset(250, "gram"));
    preset450gBtn.addEventListener("click", () => applyPreset(450, "gram"));
    preset500gBtn.addEventListener("click", () => applyPreset(500, "gram"));
    preset1kgBtn.addEventListener("click", () => applyPreset(1, "full"));
    preset2kgBtn.addEventListener("click", () => applyPreset(2, "full"));
    preset5kgBtn.addEventListener("click", () => applyPreset(5, "full"));

    function recordUdhariEntry(customerName, amountToAdd, itemNote, phoneNum) {
      const existing = udhariList.find(
        (entry) => entry.customer.toLowerCase() === customerName.toLowerCase()
      );

      const cleaned = cleanPhone(phoneNum);

      if (existing) {
        existing.amount += amountToAdd;
        if (itemNote) {
          existing.note = itemNote;
        }
        if (cleaned) {
          existing.phone = cleaned;
        }
        if (!Array.isArray(existing.history)) existing.history = [];
        existing.history.unshift(`🔴 +₹${amountToAdd} और उधारी (${itemNote || "Saman"})`);
      } else {
        udhariList.unshift({
          id: Date.now(),
          customer: customerName,
          phone: cleaned,
          amount: amountToAdd,
          note: itemNote || "Dukan saman",
          history: [`🔴 ₹${amountToAdd} उधारी शुरू (${itemNote || "Dukan saman"})`]
        });
      }

      saveToStorage();
      renderUdhari();
      renderSheetDashboard();
    }

    sendCalcToUdhariBtn.addEventListener("click", () => {
      const customer = calcCustomerInput.value.trim();
      const details = getCalculatedDetails();

      if (!customer) {
        calcCustomerInput.focus();
        showToast("⚠️ Kripya grahak ka naam likhein!");
        return;
      }
      if (!details.item || details.total <= 0) {
        showToast("⚠️ Sahi matra (wajan) dalein!");
        return;
      }

      const amount = Math.round(details.total);
      recordUdhariEntry(customer, amount, details.label, "");
      calcCustomerInput.value = "";
      showToast(`📒 ${customer} ke khate me ₹${amount} (${details.label}) jud gaya!`);
      switchTab("udhari");
    });

    // 7. MULTI-ITEM PARCHI BILL + AAJ KA GALLA
    function populateBillSelect() {
      const currentVal = billItemSelect.value;
      billItemSelect.innerHTML = "";

      if (items.length === 0) {
        const opt = document.createElement("option");
        opt.value = "";
        opt.textContent = "Pehle Rate List me saman jodein";
        billItemSelect.appendChild(opt);
        return;
      }

      items.forEach((item) => {
        const opt = document.createElement("option");
        opt.value = String(item.id);
        opt.textContent = `${item.name} (₹${item.price}/${item.unit})`;
        billItemSelect.appendChild(opt);
      });

      if (currentVal && items.some((i) => String(i.id) === currentVal)) {
        billItemSelect.value = currentVal;
      }
    }

    function renderBill() {
      billCountBadge.textContent = String(billItems.length);
      const grandTotal = Math.round(billItems.reduce((sum, b) => sum + b.price, 0));
      billGrandTotal.textContent = `₹${grandTotal}`;

      billItemsContainer.innerHTML = "";

      if (billItems.length === 0) {
        billEmptyState.classList.remove("hidden");
        return;
      }

      billEmptyState.classList.add("hidden");

      billItems.forEach((bItem) => {
        const row = document.createElement("div");
        row.className = "py-2 flex items-center justify-between text-xs sm:text-sm";

        const left = document.createElement("div");
        left.className = "font-medium text-slate-800";
        left.textContent = bItem.label;

        const right = document.createElement("div");
        right.className = "flex items-center gap-2";

        const priceSpan = document.createElement("span");
        priceSpan.className = "font-bold text-slate-900";
        priceSpan.textContent = `₹${bItem.price}`;

        const removeBtn = document.createElement("button");
        removeBtn.type = "button";
        removeBtn.className = "text-rose-500 hover:text-rose-700 font-bold px-1.5";
        removeBtn.textContent = "✕";
        removeBtn.addEventListener("click", () => {
          billItems = billItems.filter((x) => x.id !== bItem.id);
          renderBill();
        });

        right.appendChild(priceSpan);
        right.appendChild(removeBtn);
        row.appendChild(left);
        row.appendChild(right);
        billItemsContainer.appendChild(row);
      });
    }

    function renderSalesLog() {
      const totalToday = salesLog.reduce((sum, s) => sum + s.amount, 0);
      todaySalesCount.textContent = `₹${totalToday}`;

      salesLogContainer.innerHTML = "";
      if (salesLog.length === 0) {
        salesLogContainer.innerHTML = `<div class="text-slate-400 py-2">Aaj abhi tak koi nagad parchi save nahi hui hai.</div>`;
        renderSheetDashboard();
        return;
      }

      salesLog.slice(0, 6).forEach((s, idx) => {
        const item = document.createElement("div");
        item.className = "bg-emerald-50/70 border border-emerald-200/80 px-3 py-2 rounded-xl flex items-center justify-between gap-2";
        item.innerHTML = `
          <div>
            <span class="font-semibold text-emerald-900">Parchi #${salesLog.length - idx}:</span>
            <span class="text-slate-600">${escapeHtml(s.summary)}</span>
          </div>
          <span class="font-bold text-emerald-700 shrink-0">+₹${s.amount}</span>
        `;
        salesLogContainer.appendChild(item);
      });
      renderSheetDashboard();
    }

    addToBillForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const selectedId = Number(billItemSelect.value);
      const selectedItem = items.find((i) => i.id === selectedId) || items[0];
      const qty = parseFloat(billQtyInput.value) || 0;
      const mode = billUnitMode.value;

      const calc = calculateItemPrice(selectedItem, qty, mode);
      if (!calc.item || calc.total <= 0) return;

      billItems.push({
        id: Date.now() + Math.random(),
        label: calc.label,
        price: Math.round(calc.total)
      });

      billQtyInput.value = "1";
      renderBill();
      showToast(`🧾 "${calc.label}" (₹${Math.round(calc.total)}) parchi me jud gaya!`);
    });

    copyBillBtn.addEventListener("click", () => {
      if (billItems.length === 0) {
        showToast("⚠️ Parchi abhi khali hai!");
        return;
      }
      const grandTotal = Math.round(billItems.reduce((sum, b) => sum + b.price, 0));
      const lines = billItems.map((b, i) => `${i + 1}. ${b.label} - ₹${b.price}`).join("\n");
      const receiptText = `🧾 *${shopName} - Parchi*\n${lines}\n------------------\n*Kul Bill: ₹${grandTotal}*`;
      copyTextToClipboard(receiptText);
      showToast("📲 Parchi copy ho gayi! Ab WhatsApp me paste kar sakte hain.");
    });

    clearBillBtn.addEventListener("click", () => {
      billItems = [];
      renderBill();
      showToast("🧹 Parchi saaf ho gayi!");
    });

    recordCashSaleBtn.addEventListener("click", () => {
      if (billItems.length === 0) {
        showToast("⚠️ Pehle parchi me kuch saman jodein!");
        return;
      }
      const totalAmount = Math.round(billItems.reduce((sum, b) => sum + b.price, 0));
      const summaryNote = billItems.map((b) => b.label).join(", ");

      salesLog.unshift({
        id: Date.now(),
        amount: totalAmount,
        summary: summaryNote
      });
      billItems = [];
      saveToStorage();
      renderBill();
      renderSalesLog();
      showToast(`💵 ₹${totalAmount} ki Nagad Bikri Aaj ke Galla me jud gayi!`);
    });

    resetSalesBtn.addEventListener("click", () => {
      salesLog = [];
      saveToStorage();
      renderSalesLog();
      showToast("🔄 Aaj ka Nagad Galla ₹0 kar diya gaya!");
    });

    sendBillToUdhariBtn.addEventListener("click", () => {
      if (billItems.length === 0) {
        showToast("⚠️ Pehle parchi me kuch saman jodein!");
        return;
      }
      const customer = billCustomerInput.value.trim();
      if (!customer) {
        billCustomerInput.focus();
        showToast("⚠️ Kripya grahak ka naam likhein!");
        return;
      }

      const totalAmount = Math.round(billItems.reduce((sum, b) => sum + b.price, 0));
      const summaryNote = billItems.map((b) => b.label).join(", ");

      recordUdhariEntry(customer, totalAmount, summaryNote, "");
      billItems = [];
      billCustomerInput.value = "";
      renderBill();
      showToast(`📒 ${customer} ka ₹${totalAmount} ka pura bill Udhari Khata me jud gaya!`);
      switchTab("udhari");
    });

    // 8. RENDER CLEAR UDHARI KHATA LIST
    function buildReminderMessage(entry) {
      const notePart = entry.note ? ` (${entry.note})` : "";
      return `🙏 Namaste ${entry.customer} ji, ${shopName} se yaad dila rahe hain ki aapka ₹${entry.amount}${notePart} ka hisaab baki hai. Kripya suvidha anusar jama kar dein. Dhanyawad!`;
    }

    function renderUdhari() {
      const totalDue = udhariList.reduce((sum, entry) => sum + entry.amount, 0);
      totalUdhariCount.textContent = `₹${totalDue}`;
      udhariCustomerBadge.textContent = udhariList.length;

      const q = udhariSearchQuery.toLowerCase().trim();
      const filtered = udhariList.filter((entry) =>
        entry.customer.toLowerCase().includes(q) ||
        (entry.note && entry.note.toLowerCase().includes(q)) ||
        (entry.phone && entry.phone.includes(q))
      );

      udhariListContainer.innerHTML = "";

      if (filtered.length === 0) {
        udhariEmptyState.classList.remove("hidden");
        renderSheetDashboard();
        return;
      }

      udhariEmptyState.classList.add("hidden");

      filtered.forEach((entry) => {
        const card = document.createElement("div");
        card.className = "p-4 bg-slate-50/80 border border-slate-200 rounded-2xl space-y-3 transition";

        const topRow = document.createElement("div");
        topRow.className = "flex items-start justify-between gap-3";

        const leftDiv = document.createElement("div");
        const phoneDisplay = entry.phone
          ? `<span class="text-emerald-700 font-semibold">📞 ${escapeHtml(entry.phone)}</span>`
          : `<span class="text-slate-400">📞 Mobile number nahi juda</span>`;

        leftDiv.innerHTML = `
          <div class="font-bold text-slate-900 text-base">👤 ${escapeHtml(entry.customer)}</div>
          <div class="text-xs text-slate-600 mt-0.5">📦 आइटम/नोट: <span class="font-medium">${entry.note ? escapeHtml(entry.note) : "Dukan saman"}</span></div>
          <div class="text-xs mt-0.5">${phoneDisplay}</div>
        `;

        const balanceBox = document.createElement("div");
        balanceBox.className = "bg-rose-50 border border-rose-200 px-3 py-1.5 rounded-xl text-right shrink-0";
        balanceBox.innerHTML = `
          <div class="text-[11px] font-semibold text-rose-700">कुल बाकी (Lene Hain)</div>
          <div class="text-lg font-extrabold text-rose-700">₹${entry.amount}</div>
        `;

        topRow.appendChild(leftDiv);
        topRow.appendChild(balanceBox);

        const historyBox = document.createElement("div");
        historyBox.className = "bg-white px-3 py-2 rounded-xl border border-slate-200/80 text-xs text-slate-600 space-y-1";
        const recentHistory = (entry.history || []).slice(0, 3);
        historyBox.innerHTML = `<div class="font-semibold text-slate-500 text-[11px]">📜 पिछला हिसाब (Len-Den):</div>` +
          recentHistory.map((h) => `<div>• ${escapeHtml(h)}</div>`).join("");

        const customActionRow = document.createElement("div");
        customActionRow.className = "flex flex-wrap items-center gap-2 pt-1";

        const amountInput = document.createElement("input");
        amountInput.type = "number";
        amountInput.min = "1";
        amountInput.step = "any";
        amountInput.placeholder = "₹ रकम लिखें (जैसे 100)";
        amountInput.className = "w-36 sm:w-40 px-3 py-1.5 rounded-xl border border-slate-300 bg-white text-xs sm:text-sm font-semibold focus:outline-none focus:ring-2 focus:ring-amber-500";

        const jamaBtn = document.createElement("button");
        jamaBtn.type = "button";
        jamaBtn.className = "text-xs bg-emerald-600 hover:bg-emerald-700 text-white font-semibold px-3 py-2 rounded-xl transition";
        jamaBtn.textContent = "🟢 पैसे मिले (- Jama)";
        jamaBtn.addEventListener("click", () => {
          const val = parseFloat(amountInput.value);
          if (isNaN(val) || val <= 0) {
            amountInput.focus();
            showToast("⚠️ Pehle डिब्बे में रकम लिखें (जैसे 100)!");
            return;
          }
          adjustUdhari(entry.id, -val, "नकद जमा मिले");
        });

        const aurUdhariBtn = document.createElement("button");
        aurUdhariBtn.type = "button";
        aurUdhariBtn.className = "text-xs bg-rose-600 hover:bg-rose-700 text-white font-semibold px-3 py-2 rounded-xl transition";
        aurUdhariBtn.textContent = "🔴 और उधारी (+ Diye)";
        aurUdhariBtn.addEventListener("click", () => {
          const val = parseFloat(amountInput.value);
          if (isNaN(val) || val <= 0) {
            amountInput.focus();
            showToast("⚠️ Pehle डिब्बे में रकम लिखें (जैसे 50)!");
            return;
          }
          adjustUdhari(entry.id, val, "और सामान लिया");
        });

        customActionRow.appendChild(amountInput);
        customActionRow.appendChild(jamaBtn);
        customActionRow.appendChild(aurUdhariBtn);

        const bottomRow = document.createElement("div");
        bottomRow.className = "flex flex-wrap items-center justify-between gap-2 pt-2 border-t border-slate-200/80";

        const leftBtns = document.createElement("div");
        leftBtns.className = "flex flex-wrap items-center gap-1.5";

        const waMsg = encodeURIComponent(buildReminderMessage(entry));
        const cleanNum = cleanPhone(entry.phone);
        const waHref = cleanNum.length === 10
          ? `https://wa.me/91${cleanNum}?text=${waMsg}`
          : `https://wa.me/?text=${waMsg}`;

        const whatsappLink = document.createElement("a");
        whatsappLink.href = waHref;
        whatsappLink.target = "_blank";
        whatsappLink.rel = "noopener noreferrer";
        whatsappLink.className = "text-xs bg-green-600 hover:bg-green-700 text-white font-semibold px-3 py-1.5 rounded-lg transition inline-flex items-center gap-1";
        whatsappLink.textContent = "💬 WhatsApp Bhejein";
        whatsappLink.addEventListener("click", () => {
          generateReminder(entry);
        });

        const reminderBtn = document.createElement("button");
        reminderBtn.type = "button";
        reminderBtn.className = "text-xs bg-sky-50 hover:bg-sky-100 text-sky-800 border border-sky-200 font-medium px-2.5 py-1.5 rounded-lg transition";
        reminderBtn.textContent = "📲 Message Copy";
        reminderBtn.addEventListener("click", () => generateReminder(entry));

        leftBtns.appendChild(whatsappLink);
        leftBtns.appendChild(reminderBtn);

        const clearBtn = document.createElement("button");
        clearBtn.type = "button";
        clearBtn.className = "text-xs bg-emerald-50 hover:bg-emerald-100 text-emerald-800 border border-emerald-300 font-semibold px-3 py-1.5 rounded-lg transition";
        clearBtn.textContent = "✅ पूरा हिसाब चुकता (0 करें)";
        clearBtn.addEventListener("click", () => settleUdhari(entry.id, entry.customer, entry.amount));

        bottomRow.appendChild(leftBtns);
        bottomRow.appendChild(clearBtn);

        card.appendChild(topRow);
        card.appendChild(historyBox);
        card.appendChild(customActionRow);
        card.appendChild(bottomRow);
        udhariListContainer.appendChild(card);
      });

      renderSheetDashboard();
    }

    function generateReminder(entry) {
      const message = buildReminderMessage(entry);
      reminderTextOutput.textContent = message;
      reminderPreviewBox.classList.remove("hidden");
      copyTextToClipboard(message);
      showToast(`📲 ${entry.customer} ke liye reminder message copy bhi ho gaya!`);
    }

    closeReminderBoxBtn.addEventListener("click", () => {
      reminderPreviewBox.classList.add("hidden");
    });

    // 9. MANDI LIST + DUKAN KHARCHA LOGIC
    function renderMandiList() {
      mandiCountBadge.textContent = String(mandiList.length);
      mandiListContainer.innerHTML = "";

      if (mandiList.length === 0) {
        mandiEmptyState.classList.remove("hidden");
        return;
      }

      mandiEmptyState.classList.add("hidden");

      mandiList.forEach((m) => {
        const row = document.createElement("div");
        row.className = "py-3 px-2 flex items-center justify-between gap-3 hover:bg-indigo-50/40 rounded-xl transition";

        const left = document.createElement("div");
        left.innerHTML = `
          <div class="font-semibold text-slate-800 text-sm md:text-base">📦 ${escapeHtml(m.name)}</div>
          <div class="text-xs text-indigo-700 font-medium">Mangwana hai: ${escapeHtml(m.qty)}</div>
        `;

        const doneBtn = document.createElement("button");
        doneBtn.type = "button";
        doneBtn.className = "text-xs bg-emerald-600 hover:bg-emerald-700 text-white font-semibold px-3 py-1.5 rounded-lg transition shrink-0";
        doneBtn.textContent = "✅ Aa Gaya (Hatayein)";
        doneBtn.addEventListener("click", () => {
          mandiList = mandiList.filter((x) => x.id !== m.id);
          saveToStorage();
          renderMandiList();
          showToast(`✅ "${m.name}" dukan me aa gaya!`);
        });

        row.appendChild(left);
        row.appendChild(doneBtn);
        mandiListContainer.appendChild(row);
      });
    }

    function quickAddToMandi(itemName) {
      const exists = mandiList.some(
        (m) => m.name.toLowerCase() === itemName.toLowerCase()
      );
      if (exists) {
        showToast(`📦 "${itemName}" pehle se Mandi List me juda hua hai!`);
        switchTab("mandi");
        return;
      }
      mandiList.unshift({
        id: Date.now(),
        name: itemName,
        qty: "1 Bori / 1 Peti (Stock mangwana hai)"
      });
      saveToStorage();
      renderMandiList();
      showToast(`📦 "${itemName}" Mandi List me jud gaya!`);
    }

    addMandiForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const name = mandiItemInput.value.trim();
      const qty = mandiQtyInput.value.trim();
      if (!name || !qty) return;

      mandiList.unshift({
        id: Date.now(),
        name: name,
        qty: qty
      });

      mandiItemInput.value = "";
      mandiQtyInput.value = "";
      mandiItemInput.focus();

      saveToStorage();
      renderMandiList();
      showToast(`📦 "${name} (${qty})" Mandi List me jud gaya!`);
    });

    copyMandiBtn.addEventListener("click", () => {
      if (mandiList.length === 0) {
        showToast("⚠️ Mandi List abhi khali hai!");
        return;
      }
      const lines = mandiList.map((m, idx) => `${idx + 1}. ${m.name} — ${m.qty}`).join("\n");
      const text = `📦 *${shopName} - Mandi Saman Order List*\n${lines}`;
      copyTextToClipboard(text);
      showToast("📲 Mandi Order List copy ho gayi! Mahajan ko WhatsApp par bhej dein.");
    });

    function renderKharchaList() {
      const totalKharcha = kharchaList.reduce((sum, k) => sum + k.amount, 0);
      totalKharchaBadge.textContent = `₹${totalKharcha}`;

      kharchaListContainer.innerHTML = "";
      if (kharchaList.length === 0) {
        kharchaListContainer.innerHTML = `<div class="text-xs text-slate-400 py-3 text-center">Abhi koi dukan kharcha darj nahi hai.</div>`;
        renderSheetDashboard();
        return;
      }

      kharchaList.forEach((k) => {
        const row = document.createElement("div");
        row.className = "py-2.5 px-2 flex items-center justify-between gap-2 text-xs sm:text-sm";

        const left = document.createElement("div");
        left.className = "font-medium text-slate-800";
        left.textContent = `💸 ${k.title}`;

        const right = document.createElement("div");
        right.className = "flex items-center gap-2";

        const amt = document.createElement("span");
        amt.className = "font-bold text-amber-900 bg-amber-50 border border-amber-200 px-2.5 py-1 rounded-lg";
        amt.textContent = `₹${k.amount}`;

        const delBtn = document.createElement("button");
        delBtn.type = "button";
        delBtn.className = "text-rose-500 hover:text-rose-700 font-bold px-1.5";
        delBtn.textContent = "✕";
        delBtn.addEventListener("click", () => {
          kharchaList = kharchaList.filter((x) => x.id !== k.id);
          saveToStorage();
          renderKharchaList();
          showToast("🗑️ Kharcha hata diya gaya.");
        });

        right.appendChild(amt);
        right.appendChild(delBtn);
        row.appendChild(left);
        row.appendChild(right);
        kharchaListContainer.appendChild(row);
      });

      renderSheetDashboard();
    }

    addKharchaForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const title = kharchaTitleInput.value.trim();
      const amount = parseFloat(kharchaAmountInput.value);
      if (!title || isNaN(amount) || amount <= 0) return;

      kharchaList.unshift({
        id: Date.now(),
        title: title,
        amount: amount
      });

      kharchaTitleInput.value = "";
      kharchaAmountInput.value = "";
      kharchaTitleInput.focus();

      saveToStorage();
      renderKharchaList();
      showToast(`💸 "${title}" (₹${amount}) Dukan Kharcha me jud gaya!`);
    });

    // 10. PROFIT, UDHARI, LOSS & GOOGLE SHEET SYNC LOGIC
    function getLiveSummaryStats() {
      const liveSales = salesLog.reduce((s, e) => s + e.amount, 0);
      const liveLoss = kharchaList.reduce((s, k) => s + k.amount, 0);
      const liveUdhari = udhariList.reduce((s, u) => s + u.amount, 0);
      const liveNet = liveSales - liveLoss;
      return { liveSales, liveLoss, liveUdhari, liveNet };
    }

    function renderSheetDashboard() {
      const { liveSales, liveLoss, liveUdhari, liveNet } = getLiveSummaryStats();

      sheetSalesStat.textContent = `₹${liveSales}`;
      sheetLossStat.textContent = `₹${liveLoss}`;
      sheetUdhariStat.textContent = `₹${liveUdhari}`;

      const sign = liveNet >= 0 ? "+" : "-";
      const formattedNet = `${sign}₹${Math.abs(liveNet)}`;
      sheetNetStat.textContent = formattedNet;
      headerNetProfitCount.textContent = formattedNet;

      if (liveNet >= 0) {
        sheetNetLabel.textContent = "📈 शुद्ध मुनाफा (Profit)";
        sheetNetCard.className = "bg-indigo-50 border border-indigo-200 p-3 rounded-xl";
        sheetNetStat.className = "text-lg font-extrabold text-indigo-700 mt-0.5";
      } else {
        sheetNetLabel.textContent = "📉 नुकसान (Net Loss)";
        sheetNetCard.className = "bg-rose-50 border border-rose-300 p-3 rounded-xl";
        sheetNetStat.className = "text-lg font-extrabold text-rose-700 mt-0.5";
      }

      sheetRowsContainer.innerHTML = "";
      sheetRows.forEach((r) => {
        const tr = document.createElement("tr");
        tr.className = "hover:bg-slate-50";

        const netSign = r.net >= 0 ? "+" : "-";
        const netColor = r.net >= 0 ? "text-emerald-700 font-bold" : "text-rose-700 font-bold";

        tr.innerHTML = `
          <td class="p-2.5 font-medium text-slate-800">${escapeHtml(r.date)}</td>
          <td class="p-2.5 text-emerald-700 font-semibold">₹${r.profit}</td>
          <td class="p-2.5 text-rose-700 font-semibold">₹${r.udhari}</td>
          <td class="p-2.5 text-amber-800 font-semibold">₹${r.loss}</td>
          <td class="p-2.5 ${netColor}">${netSign}₹${Math.abs(r.net)}</td>
          <td class="p-2.5 text-right"></td>
        `;

        const delBtn = document.createElement("button");
        delBtn.type = "button";
        delBtn.className = "text-rose-500 hover:text-rose-700 font-bold px-1.5";
        delBtn.textContent = "✕";
        delBtn.addEventListener("click", () => {
          sheetRows = sheetRows.filter((x) => x.id !== r.id);
          saveToStorage();
          renderSheetDashboard();
        });

        tr.lastElementChild.appendChild(delBtn);
        sheetRowsContainer.appendChild(tr);
      });
    }

    saveCurrentToSheetTableBtn.addEventListener("click", () => {
      const { liveSales, liveLoss, liveUdhari, liveNet } = getLiveSummaryStats();
      const todayStr = new Date().toLocaleDateString("en-IN", { day: "2-digit", month: "short" });

      sheetRows.unshift({
        id: Date.now(),
        date: todayStr,
        profit: liveSales,
        udhari: liveUdhari,
        loss: liveLoss,
        net: liveNet
      });

      saveToStorage();
      renderSheetDashboard();
      showToast("📊 Aaj ka Profit, Udhari aur Loss Google Sheet Table me jud gaya!");
    });

    addSheetRowForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const dateVal = sheetDateInput.value.trim() || "Aaj";
      const profitVal = parseFloat(sheetProfitInput.value) || 0;
      const udhariVal = parseFloat(sheetUdhariInput.value) || 0;
      const lossVal = parseFloat(sheetLossInput.value) || 0;
      const netVal = profitVal - lossVal;

      sheetRows.unshift({
        id: Date.now(),
        date: dateVal,
        profit: profitVal,
        udhari: udhariVal,
        loss: lossVal,
        net: netVal
      });

      sheetDateInput.value = "";
      sheetProfitInput.value = "";
      sheetUdhariInput.value = "";
      sheetLossInput.value = "";

      saveToStorage();
      renderSheetDashboard();
      showToast(`📊 "${dateVal}" ka hisab Sheet Table me jud gaya!`);
    });

    copyForSheetBtn.addEventListener("click", () => {
      const header = ["Date", "Shop Name", "Profit / Sales (Rs)", "Udhari Baki (Rs)", "Loss / Kharcha (Rs)", "Net Profit/Loss (Rs)"].join("\t");
      const lines = sheetRows.map((r) =>
        [r.date, shopName, r.profit, r.udhari, r.loss, r.net].join("\t")
      );
      const tsv = [header, ...lines].join("\n");
      copyTextToClipboard(tsv);
      showToast("📋 Google Sheet ke liye Table Copy ho gayi! Sheet me जाकर Paste (Ctrl+V) kar dein.");
    });

    downloadCsvBtn.addEventListener("click", () => {
      const header = "Date,Shop Name,Profit_Sales_INR,Udhari_Baki_INR,Loss_Kharcha_INR,Net_Profit_Loss_INR";
      const lines = sheetRows.map((r) =>
        `"${r.date}","${shopName}",${r.profit},${r.udhari},${r.loss},${r.net}`
      );
      const csvContent = [header, ...lines].join("\n");
      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "dukan_profit_udhari_loss_sheet.csv";
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
      showToast("📥 Google Sheet (.csv) file download ho gayi!");
    });

    toggleScriptGuideBtn.addEventListener("click", () => {
      scriptGuideBox.classList.toggle("hidden");
    });

    copyAppsScriptBtn.addEventListener("click", () => {
      copyTextToClipboard(appsScriptCodeText.textContent);
      showToast("📋 Google Apps Script code copy ho gaya!");
    });

    sheetWebhookInput.addEventListener("input", (e) => {
      sheetWebhookUrl = e.target.value.trim();
      saveToStorage();
    });

    sendToGoogleSheetBtn.addEventListener("click", async () => {
      const url = sheetWebhookInput.value.trim();
      if (!url || !url.startsWith("https://")) {
        sheetWebhookInput.focus();
        scriptGuideBox.classList.remove("hidden");
        showToast("⚠️ Pehle Google Apps Script URL dalein (ya Copy/CSV button use karein)!");
        return;
      }

      const latest = sheetRows[0] || {
        date: new Date().toLocaleDateString("en-IN", { day: "2-digit", month: "short" }),
        profit: salesLog.reduce((s, e) => s + e.amount, 0),
        udhari: udhariList.reduce((s, u) => s + u.amount, 0),
        loss: kharchaList.reduce((s, k) => s + k.amount, 0),
        net: salesLog.reduce((s, e) => s + e.amount, 0) - kharchaList.reduce((s, k) => s + k.amount, 0)
      };

      const payload = {
        date: latest.date,
        shop: shopName,
        profit: latest.profit,
        udhari: latest.udhari,
        loss: latest.loss,
        net: latest.net,
        note: `Udhari Customers: ${udhariList.length}`
      };

      try {
        await fetch(url, {
          method: "POST",
          mode: "no-cors",
          headers: { "Content-Type": "text/plain;charset=utf-8" },
          body: JSON.stringify(payload)
        });
        showToast("🚀 Hisab seedhe aapki Google Sheet me bhej diya gaya!");
      } catch (err) {
        showToast("⚠️ Network rukawat: Kripya 'Copy for Google Sheet' ya 'Download CSV' use karein.");
      }
    });

    // 11. ACTIONS FOR SAMAN RATE LIST
    addItemForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const name = itemNameInput.value.trim();
      const price = parseFloat(itemPriceInput.value);
      const unit = itemUnitInput.value;
      const category = itemCategoryInput.value || "anaj";

      if (!name || isNaN(price) || price <= 0) return;

      items.unshift({
        id: Date.now(),
        name: name,
        price: price,
        unit: unit,
        category: category
      });

      itemNameInput.value = "";
      itemPriceInput.value = "";
      itemNameInput.focus();

      saveToStorage();
      renderItems();
      showToast(`✅ "${name}" (₹${price}/${unit}) save ho gaya!`);
    });

    function deleteItem(id, name) {
      items = items.filter((item) => item.id !== id);
      saveToStorage();
      renderItems();
      showToast(`🗑️ "${name}" list se hata diya gaya.`);
    }

    searchInput.addEventListener("input", (e) => {
      searchQuery = e.target.value;
      if (searchQuery.length > 0) {
        clearSearchBtn.classList.remove("hidden");
      } else {
        clearSearchBtn.classList.add("hidden");
      }
      renderItems();
    });

    clearSearchBtn.addEventListener("click", () => {
      searchInput.value = "";
      searchQuery = "";
      clearSearchBtn.classList.add("hidden");
      renderItems();
    });

    // 12. ACTIONS FOR UDHARI KHATA
    addUdhariForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const customer = customerNameInput.value.trim();
      const phone = customerPhoneInput.value.trim();
      const amount = parseFloat(udhariAmountInput.value);
      const note = udhariNoteInput.value.trim();

      if (!customer || isNaN(amount) || amount <= 0) return;

      recordUdhariEntry(customer, amount, note || "Dukan saman", phone);
      showToast(`📒 "${customer}" ke khate me ₹${amount} darj ho gaye!`);

      customerNameInput.value = "";
      customerPhoneInput.value = "";
      udhariAmountInput.value = "";
      udhariNoteInput.value = "";
      customerNameInput.focus();
    });

    function adjustUdhari(id, delta, reason) {
      const target = udhariList.find((entry) => entry.id === id);
      if (!target) return;

      target.amount += delta;
      if (!Array.isArray(target.history)) target.history = [];

      if (target.amount <= 0) {
        udhariList = udhariList.filter((entry) => entry.id !== id);
        showToast(`🎉 ${target.customer} ka pura hisaab chukta ho gaya!`);
      } else {
        if (delta > 0) {
          target.history.unshift(`🔴 +₹${delta} और उधारी (${reason || "Saman"}) → अब बाकी: ₹${target.amount}`);
          showToast(`🔴 ${target.customer}: ₹${delta} aur jode gaye (Ab Baki: ₹${target.amount})`);
        } else {
          target.history.unshift(`🟢 -₹${Math.abs(delta)} जमा मिले → अब बाकी: ₹${target.amount}`);
          showToast(`🟢 ${target.customer}: ₹${Math.abs(delta)} jama mile (Ab Baki: ₹${target.amount})`);
        }
      }
      saveToStorage();
      renderUdhari();
    }

    function settleUdhari(id, customer, amount) {
      udhariList = udhariList.filter((entry) => entry.id !== id);
      saveToStorage();
      renderUdhari();
      showToast(`✅ ${customer} ke ₹${amount} pura chukta ho gaye!`);
    }

    udhariSearchInput.addEventListener("input", (e) => {
      udhariSearchQuery = e.target.value;
      renderUdhari();
    });

    downloadBackupBtn.addEventListener("click", () => {
      const totalDue = udhariList.reduce((s, e) => s + e.amount, 0);
      const totalSales = salesLog.reduce((s, e) => s + e.amount, 0);
      const totalKharcha = kharchaList.reduce((s, k) => s + k.amount, 0);
      const itemLines = items.map((i) => `- ${i.name} [${CATEGORY_LABELS[i.category] || ""}]: ₹${i.price} / ${i.unit}`).join("\n");
      const udhariLines = udhariList
        .map((u) => `- ${u.customer} (${u.phone || "No Phone"}): ₹${u.amount} Baki [${u.note || ""}]`)
        .join("\n");
      const mandiLines = mandiList
        .map((m) => `- ${m.name}: ${m.qty}`)
        .join("\n");
      const kharchaLines = kharchaList
        .map((k) => `- ${k.title}: ₹${k.amount}`)
        .join("\n");

      const content = [
        `=== ${shopName.toUpperCase()} - BACKUP REPORT ===`,
        `Kul Saman: ${items.length} | Aaj ki Nagad Bikri: ₹${totalSales} | Kul Udhari Baki: ₹${totalDue} | Kul Kharcha: ₹${totalKharcha}`,
        "",
        "--- 1. SAMAN RATE LIST ---",
        itemLines || "Koi saman nahi",
        "",
        "--- 2. GRAHAK UDHARI KHATA ---",
        udhariLines || "Koi udhari baki nahi",
        "",
        "--- 3. MANDI ORDER LIST ---",
        mandiLines || "Koi saman baki nahi",
        "",
        "--- 4. DUKAN KHARCHA ---",
        kharchaLines || "Koi kharcha nahi"
      ].join("\n");

      const blob = new Blob([content], { type: "text/plain;charset=utf-8" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "dukan_sathi_khata_backup.txt";
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
      showToast("📥 Aapke Khata, Rate, Mandi aur Kharcha ki Backup file download ho gayi!");
    });

    resetDataBtn.addEventListener("click", () => {
      items = structuredClone(DEFAULT_ITEMS);
      udhariList = structuredClone(DEFAULT_UDHARI);
      salesLog = [];
      mandiList = structuredClone(DEFAULT_MANDI);
      kharchaList = structuredClone(DEFAULT_KHARCHA);
      sheetRows = structuredClone(DEFAULT_SHEET_ROWS);
      saveToStorage();
      renderItems();
      renderSalesLog();
      renderUdhari();
      renderMandiList();
      renderKharchaList();
      renderSheetDashboard();
      showToast("🔄 Sample data wapas set ho gaya!");
    });

    // Initialize app
    loadFromStorage();
    sheetWebhookInput.value = sheetWebhookUrl;
    renderShopName();
    renderItems();
    updateDirectGramCalc();
    renderBill();
    renderSalesLog();
    renderUdhari();
    renderMandiList();
    renderKharchaList();
    renderSheetDashboard();
  </script>
</body>
</html>
