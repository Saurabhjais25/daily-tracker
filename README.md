# daily-tracker

🏗️ HTML Structure
पूरे app का skeleton यहाँ बना है:
Header — Logo, week navigation buttons (‹ ›), theme toggle, और "add task" button रखे हैं।

Tabs — तीन views के बीच switch करने के लिए: Week, Insights, और Today।

Week Tab — Quick-add templates की strip, आज के tasks की chips, और main weekly calendar grid।

Insights Tab — Charts और analytics दिखाने के लिए empty containers (JS बाद में भरता है)।

Modal — Task add/edit करने का popup form — naam, emoji, days, time, reminder, color सब यहाँ हैं।







🎨 CSS Design
Theme System — [data-theme="dark"] और [data-theme="light"] से पूरी color scheme switch होती है CSS variables (--bg, --text, --acc) के through।

Grid Layout — display:grid से weekly calendar बना है — 56px की time label column + 7 equal day columns।

Task Blocks (.tb) — position:absolute use होता है ताकि blocks सही time पर vertically place हों। Height और top, duration और start time से calculate होती है।

Animations — @keyframes fadeUp से sections smoothly आते हैं, pulse से today का dot blink करता है, pulse2 से now-line का red dot animate होता है।

Responsive — @media(max-width:640px) में mobile के लिए font sizes, grid columns, और button labels adjust होते हैं।






⚙️ JavaScript Logic
यह सबसे बड़ा हिस्सा है, इसे parts में समझते हैं:

State Management — सारा data (tasks, done, theme) एक object में रहता है जो localStorage में save होता है। App reload होने पर loadState() वापस लाता है।

render() function — यह master function है जो हर change के बाद पूरा UI दोबारा draw करता है — week header, time grid, today strip, और insights सब।

placeBlock() — हर task को grid पर physically place करता है। Start time से top calculate होती है और duration से height:

jsel.style.top = (offM / 60 * 100) + '%';
el.style.height = Math.max((dur / 60) * 44, 22) + 'px';

Drag & Drop — dragstart, dragover, drop events से task को किसी भी time slot पर drag करके move किया जा सकता है। Drop होने पर नया start/end time calculate होकर save होता है।

getBusyGrid() — 7×24 का 2D array बनाता है जिसमें हर day-hour slot के लिए busy minutes store हैं। Insights के सारे charts इसी data से बनते हैं।

Insights Rendering — Heatmap, bar chart, flow grid, और peak hours chart — सब getBusyGrid() का data लेकर dynamically HTML inject करते हैं।

Reminder System — Browser की Notification API use होती है। Task save होने पर setTimeout() से exact reminder time calculate होती है और notification schedule होती है।

Quick Templates — Pre-defined task objects (QUICK_TEMPLATES) directly state में add हो जाते हैं बिना modal खोले।

Now-Line — हर minute current time calculate होती है, उसे grid पर pixel में convert करके red line place होती है।
