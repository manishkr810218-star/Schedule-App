import { useState } from "react";

const SCHEDULE_TEMPLATE = [
  {
    day: "Monday",
    theme: "Research Day",
    color: "#00d4ff",
    icon: "🔍",
    tasks: [
      { time: "5:30–6:00 AM", task: "Read AI crime news headlines, save links", type: "morning" },
      { time: "3:00–5:30 PM", task: "Rest — listen to crime/tech podcast", type: "rest" },
      { time: "5:30–8:30 PM", task: "Deep research (Perplexity AI + Google Scholar)", type: "main" },
      { time: "8:30–11:00 PM", task: "Build script outline — Cold Open + 4 Acts", type: "main" },
    ],
  },
  {
    day: "Tuesday",
    theme: "Script Writing",
    color: "#ff6b35",
    icon: "✍️",
    tasks: [
      { time: "5:30–6:00 AM", task: "Review Monday's outline with fresh eyes", type: "morning" },
      { time: "5:30–9:30 PM", task: "Write full script — narrator lines only", type: "main" },
      { time: "9:30–11:00 PM", task: "Self-review: read aloud, fix awkward lines", type: "main" },
    ],
  },
  {
    day: "Wednesday",
    theme: "Narration Day",
    color: "#a855f7",
    icon: "🎙️",
    tasks: [
      { time: "5:30–6:00 AM", task: "Clean script — remove all stage directions", type: "morning" },
      { time: "5:30–8:00 PM", task: "Generate narration section by section in ElevenLabs", type: "main" },
      { time: "8:00–9:30 PM", task: "Review every clip, re-generate weak takes", type: "main" },
      { time: "9:30–11:00 PM", task: "Export all audio, label by Act", type: "main" },
    ],
  },
  {
    day: "Thursday",
    theme: "B-Roll + Timeline",
    color: "#10b981",
    icon: "🎬",
    tasks: [
      { time: "5:30–6:00 AM", task: "List all B-roll shots needed from script", type: "morning" },
      { time: "5:30–8:00 PM", task: "Source B-roll (Pexels, Pixabay, Artgrid)", type: "main" },
      { time: "8:00–11:00 PM", task: "Build DaVinci timeline — narration first, B-roll on top", type: "main" },
    ],
  },
  {
    day: "Friday",
    theme: "Full Editing",
    color: "#f59e0b",
    icon: "⚡",
    tasks: [
      { time: "5:30–6:00 AM", task: "Quick review of Thursday's rough timeline", type: "morning" },
      { time: "5:30–9:00 PM", task: "Edit in DaVinci — sync B-roll, kinetic text, transitions", type: "main" },
      { time: "9:00–10:00 PM", task: "Add Epidemic Sound music at -18 to -20dB", type: "main" },
      { time: "10:00–11:00 PM", task: "Color grade — teal/orange look, add film grain", type: "main" },
    ],
  },
  {
    day: "Saturday",
    theme: "Polish + SEO",
    color: "#ec4899",
    icon: "💎",
    tasks: [
      { time: "5:30–6:00 AM", task: "Note anything off from Friday's edit", type: "morning" },
      { time: "5:30–7:30 PM", task: "Final audio mix — hit -14 LUFS", type: "main" },
      { time: "7:30–9:30 PM", task: "Design thumbnail (bold number + glitch split)", type: "main" },
      { time: "9:30–11:00 PM", task: "VidIQ SEO — title, tags, description, chapters", type: "main" },
    ],
  },
  {
    day: "Sunday",
    theme: "Upload + Plan",
    color: "#06b6d4",
    icon: "🚀",
    tasks: [
      { time: "5:30–6:00 AM", task: "Final watch of completed video", type: "morning" },
      { time: "5:30–7:30 PM", task: "Upload — schedule for Tuesday 2:00 PM EST", type: "main" },
      { time: "7:30–9:00 PM", task: "Post Instagram + Facebook teaser clip", type: "main" },
      { time: "9:00–11:00 PM", task: "Start research for next week's topic", type: "main" },
    ],
  },
];

const BLOCKED = [
  { time: "6:00–7:30 AM", label: "Home Tutor", icon: "👨‍🏫" },
  { time: "8:00 AM–3:00 PM", label: "Social Media Job", icon: "💼" },
  { time: "3:00–5:30 PM", label: "Rest & Recharge", icon: "😴" },
];

const typeStyle = {
  morning: { bg: "rgba(255,255,255,0.05)", border: "rgba(255,255,255,0.1)" },
  rest: { bg: "rgba(16,185,129,0.07)", border: "rgba(16,185,129,0.2)" },
  main: { bg: "rgba(0,212,255,0.05)", border: "rgba(0,212,255,0.15)" },
};

function getWeekDates(startDate) {
  const dates = [];
  const start = new Date(startDate);
  // Adjust to Monday
  const day = start.getDay();
  const diff = day === 0 ? -6 : 1 - day;
  start.setDate(start.getDate() + diff);
  for (let i = 0; i < 7; i++) {
    const d = new Date(start);
    d.setDate(start.getDate() + i);
    dates.push(d);
  }
  return dates;
}

function formatDate(date) {
  return date.toLocaleDateString("en-US", { month: "short", day: "numeric" });
}

export default function App() {
  const today = new Date();
  const todayStr = today.toISOString().split("T")[0];
  const [selectedDate, setSelectedDate] = useState(todayStr);
  const [activeDay, setActiveDay] = useState(0);
  const [view, setView] = useState("week"); // "week" | "planner"
  const [videoTopic, setVideoTopic] = useState("");
  const [generatedPlan, setGeneratedPlan] = useState(null);

  const weekDates = getWeekDates(selectedDate);

  function generatePlan() {
    if (!videoTopic.trim()) return;
    const plan = SCHEDULE_TEMPLATE.map((d, i) => ({
      ...d,
      date: formatDate(weekDates[i]),
      fullDate: weekDates[i],
      topic: videoTopic,
    }));
    setGeneratedPlan(plan);
  }

  const currentDayData = SCHEDULE_TEMPLATE[activeDay];

  return (
    <div style={{
      minHeight: "100vh",
      background: "#080c14",
      color: "#e2e8f0",
      fontFamily: "'DM Mono', 'Courier New', monospace",
      padding: "0",
      position: "relative",
      overflow: "hidden",
    }}>
      {/* Background grid */}
      <div style={{
        position: "fixed", inset: 0, zIndex: 0,
        backgroundImage: `
          linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px),
          linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px)
        `,
        backgroundSize: "40px 40px",
        pointerEvents: "none",
      }} />

      {/* Glow orb */}
      <div style={{
        position: "fixed", top: "-200px", left: "50%", transform: "translateX(-50%)",
        width: "600px", height: "400px",
        background: "radial-gradient(ellipse, rgba(0,212,255,0.06) 0%, transparent 70%)",
        zIndex: 0, pointerEvents: "none",
      }} />

      <div style={{ position: "relative", zIndex: 1, maxWidth: "900px", margin: "0 auto", padding: "32px 16px" }}>

        {/* Header */}
        <div style={{ textAlign: "center", marginBottom: "40px" }}>
          <div style={{
            fontSize: "10px", letterSpacing: "6px", color: "#00d4ff",
            textTransform: "uppercase", marginBottom: "12px", opacity: 0.8,
          }}>
            ◈ PRODUCTION COMMAND CENTER ◈
          </div>
          <h1 style={{
            fontSize: "clamp(24px, 5vw, 42px)", fontWeight: "900",
            background: "linear-gradient(135deg, #ffffff 0%, #00d4ff 50%, #a855f7 100%)",
            WebkitBackgroundClip: "text", WebkitTextFillColor: "transparent",
            letterSpacing: "-1px", lineHeight: 1.1, marginBottom: "8px",
            fontFamily: "'Georgia', serif",
          }}>
            SHADOWS IN THE MACHINE
          </h1>
          <p style={{ color: "#64748b", fontSize: "12px", letterSpacing: "3px", textTransform: "uppercase" }}>
            Weekly Schedule System · Manish
          </p>
        </div>

        {/* Nav */}
        <div style={{ display: "flex", gap: "8px", marginBottom: "32px", justifyContent: "center" }}>
          {["week", "planner"].map(v => (
            <button key={v} onClick={() => setView(v)} style={{
              padding: "10px 28px", borderRadius: "2px", cursor: "pointer",
              border: view === v ? "1px solid #00d4ff" : "1px solid rgba(255,255,255,0.08)",
              background: view === v ? "rgba(0,212,255,0.1)" : "transparent",
              color: view === v ? "#00d4ff" : "#64748b",
              fontSize: "11px", letterSpacing: "3px", textTransform: "uppercase",
              fontFamily: "inherit", transition: "all 0.2s",
            }}>
              {v === "week" ? "⬡ This Week" : "◈ Future Planner"}
            </button>
          ))}
        </div>

        {view === "week" && (
          <>
            {/* Blocked time banner */}
            <div style={{
              background: "rgba(255,255,255,0.02)", border: "1px solid rgba(255,255,255,0.06)",
              borderRadius: "4px", padding: "16px 20px", marginBottom: "28px",
            }}>
              <div style={{ fontSize: "9px", letterSpacing: "4px", color: "#64748b", marginBottom: "12px", textTransform: "uppercase" }}>
                Daily Blocked Hours (Every Day)
              </div>
              <div style={{ display: "flex", gap: "12px", flexWrap: "wrap" }}>
                {BLOCKED.map((b, i) => (
                  <div key={i} style={{
                    display: "flex", alignItems: "center", gap: "8px",
                    background: "rgba(255,255,255,0.03)", border: "1px solid rgba(255,255,255,0.07)",
                    borderRadius: "2px", padding: "8px 14px",
                  }}>
                    <span>{b.icon}</span>
                    <div>
                      <div style={{ fontSize: "10px", color: "#475569" }}>{b.time}</div>
                      <div style={{ fontSize: "12px", color: "#94a3b8" }}>{b.label}</div>
                    </div>
                  </div>
                ))}
              </div>
            </div>

            {/* Day selector tabs */}
            <div style={{ display: "flex", gap: "6px", marginBottom: "24px", overflowX: "auto", paddingBottom: "4px" }}>
              {SCHEDULE_TEMPLATE.map((d, i) => (
                <button key={i} onClick={() => setActiveDay(i)} style={{
                  flex: "0 0 auto", padding: "10px 16px", borderRadius: "2px",
                  cursor: "pointer", border: activeDay === i
                    ? `1px solid ${d.color}`
                    : "1px solid rgba(255,255,255,0.06)",
                  background: activeDay === i
                    ? `rgba(${hexToRgb(d.color)},0.1)`
                    : "rgba(255,255,255,0.02)",
                  color: activeDay === i ? d.color : "#475569",
                  fontSize: "11px", fontFamily: "inherit", transition: "all 0.2s",
                  textAlign: "center",
                }}>
                  <div style={{ fontSize: "16px", marginBottom: "2px" }}>{d.icon}</div>
                  <div style={{ letterSpacing: "1px", fontSize: "10px" }}>{d.day.slice(0, 3).toUpperCase()}</div>
                </button>
              ))}
            </div>

            {/* Active Day Detail */}
            <div style={{
              border: `1px solid ${currentDayData.color}30`,
              borderLeft: `3px solid ${currentDayData.color}`,
              borderRadius: "4px", padding: "28px",
              background: `rgba(${hexToRgb(currentDayData.color)},0.04)`,
            }}>
              <div style={{ display: "flex", alignItems: "center", gap: "16px", marginBottom: "24px" }}>
                <span style={{ fontSize: "32px" }}>{currentDayData.icon}</span>
                <div>
                  <div style={{ fontSize: "10px", letterSpacing: "4px", color: currentDayData.color, textTransform: "uppercase" }}>
                    {currentDayData.day}
                  </div>
                  <div style={{ fontSize: "22px", fontWeight: "700", color: "#f1f5f9", fontFamily: "'Georgia', serif" }}>
                    {currentDayData.theme}
                  </div>
                </div>
              </div>

              <div style={{ display: "flex", flexDirection: "column", gap: "10px" }}>
                {currentDayData.tasks.map((task, i) => (
                  <div key={i} style={{
                    display: "flex", gap: "16px", alignItems: "flex-start",
                    padding: "14px 16px", borderRadius: "2px",
                    background: typeStyle[task.type].bg,
                    border: `1px solid ${typeStyle[task.type].border}`,
                  }}>
                    <div style={{
                      minWidth: "110px", fontSize: "10px", color: currentDayData.color,
                      letterSpacing: "1px", paddingTop: "2px",
                    }}>
                      {task.time}
                    </div>
                    <div style={{ fontSize: "13px", color: "#cbd5e1", lineHeight: "1.5" }}>
                      {task.task}
                    </div>
                  </div>
                ))}
              </div>
            </div>

            {/* Weekly stats */}
            <div style={{
              display: "grid", gridTemplateColumns: "repeat(3, 1fr)", gap: "12px", marginTop: "24px",
            }}>
              {[
                { label: "Active hrs/day", value: "5.5h", sub: "5:30–11:00 PM" },
                { label: "Total hrs/week", value: "22.5h", sub: "YouTube work" },
                { label: "Upload target", value: "Tue 2PM", sub: "EST · Weekly" },
              ].map((stat, i) => (
                <div key={i} style={{
                  border: "1px solid rgba(255,255,255,0.06)", borderRadius: "4px",
                  padding: "16px", textAlign: "center",
                  background: "rgba(255,255,255,0.02)",
                }}>
                  <div style={{ fontSize: "22px", fontWeight: "800", color: "#00d4ff", fontFamily: "'Georgia', serif" }}>
                    {stat.value}
                  </div>
                  <div style={{ fontSize: "10px", color: "#64748b", marginTop: "4px", letterSpacing: "1px" }}>
                    {stat.label}
                  </div>
                  <div style={{ fontSize: "9px", color: "#475569", marginTop: "2px" }}>
                    {stat.sub}
                  </div>
                </div>
              ))}
            </div>
          </>
        )}

        {view === "planner" && (
          <>
            {/* Topic input */}
            <div style={{
              border: "1px solid rgba(0,212,255,0.2)", borderRadius: "4px",
              padding: "24px", marginBottom: "24px",
              background: "rgba(0,212,255,0.03)",
            }}>
              <div style={{ fontSize: "9px", letterSpacing: "4px", color: "#00d4ff", marginBottom: "16px", textTransform: "uppercase" }}>
                ◈ Generate Future Week Schedule
              </div>

              <div style={{ display: "flex", gap: "8px", marginBottom: "16px", flexWrap: "wrap" }}>
                <div style={{ flex: 1, minWidth: "200px" }}>
                  <label style={{ fontSize: "10px", color: "#64748b", letterSpacing: "2px", display: "block", marginBottom: "8px", textTransform: "uppercase" }}>
                    Week Starting
                  </label>
                  <input
                    type="date"
                    value={selectedDate}
                    onChange={e => setSelectedDate(e.target.value)}
                    style={{
                      width: "100%", background: "rgba(255,255,255,0.04)",
                      border: "1px solid rgba(255,255,255,0.1)", borderRadius: "2px",
                      padding: "10px 14px", color: "#e2e8f0", fontSize: "13px",
                      fontFamily: "inherit", outline: "none", boxSizing: "border-box",
                    }}
                  />
                </div>
                <div style={{ flex: 2, minWidth: "200px" }}>
                  <label style={{ fontSize: "10px", color: "#64748b", letterSpacing: "2px", display: "block", marginBottom: "8px", textTransform: "uppercase" }}>
                    Video Topic
                  </label>
                  <input
                    type="text"
                    value={videoTopic}
                    onChange={e => setVideoTopic(e.target.value)}
                    placeholder="e.g. AI Deepfake Election Fraud 2025"
                    style={{
                      width: "100%", background: "rgba(255,255,255,0.04)",
                      border: "1px solid rgba(255,255,255,0.1)", borderRadius: "2px",
                      padding: "10px 14px", color: "#e2e8f0", fontSize: "13px",
                      fontFamily: "inherit", outline: "none", boxSizing: "border-box",
                    }}
                  />
                </div>
              </div>

              <button onClick={generatePlan} style={{
                padding: "12px 32px", background: "linear-gradient(135deg, #00d4ff20, #a855f720)",
                border: "1px solid #00d4ff", borderRadius: "2px", color: "#00d4ff",
                fontSize: "11px", letterSpacing: "3px", textTransform: "uppercase",
                cursor: "pointer", fontFamily: "inherit",
              }}>
                ▶ Generate Schedule
              </button>
            </div>

            {/* Generated Plan */}
            {generatedPlan && (
              <div>
                <div style={{
                  fontSize: "9px", letterSpacing: "4px", color: "#64748b",
                  marginBottom: "16px", textTransform: "uppercase", textAlign: "center",
                }}>
                  Week of {formatDate(weekDates[0])} — {formatDate(weekDates[6])} · {videoTopic}
                </div>

                <div style={{ display: "flex", flexDirection: "column", gap: "10px" }}>
                  {generatedPlan.map((day, i) => (
                    <div key={i} style={{
                      border: `1px solid ${day.color}25`,
                      borderLeft: `3px solid ${day.color}`,
                      borderRadius: "4px", padding: "16px 20px",
                      background: `rgba(${hexToRgb(day.color)},0.03)`,
                    }}>
                      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: "12px", flexWrap: "wrap", gap: "8px" }}>
                        <div style={{ display: "flex", alignItems: "center", gap: "12px" }}>
                          <span style={{ fontSize: "20px" }}>{day.icon}</span>
                          <div>
                            <span style={{ fontSize: "10px", color: day.color, letterSpacing: "2px", textTransform: "uppercase" }}>
                              {day.day}
                            </span>
                            <span style={{ fontSize: "12px", color: "#94a3b8", marginLeft: "12px" }}>
                              {day.theme}
                            </span>
                          </div>
                        </div>
                        <div style={{
                          fontSize: "10px", color: "#475569", background: "rgba(255,255,255,0.03)",
                          border: "1px solid rgba(255,255,255,0.06)", padding: "4px 10px", borderRadius: "2px",
                        }}>
                          {day.date}
                        </div>
                      </div>

                      <div style={{ display: "flex", flexDirection: "column", gap: "6px" }}>
                        {day.tasks.map((task, j) => (
                          <div key={j} style={{
                            display: "flex", gap: "12px", fontSize: "12px",
                            padding: "8px 12px", background: "rgba(255,255,255,0.02)",
                            border: "1px solid rgba(255,255,255,0.04)", borderRadius: "2px",
                          }}>
                            <span style={{ minWidth: "105px", color: day.color, fontSize: "10px", paddingTop: "1px" }}>
                              {task.time}
                            </span>
                            <span style={{ color: "#94a3b8" }}>{task.task}</span>
                          </div>
                        ))}
                      </div>
                    </div>
                  ))}
                </div>

                <div style={{
                  marginTop: "20px", padding: "16px 20px",
                  border: "1px solid rgba(16,185,129,0.2)", borderRadius: "4px",
                  background: "rgba(16,185,129,0.04)", fontSize: "12px", color: "#6ee7b7",
                  letterSpacing: "1px",
                }}>
                  ✓ Upload scheduled: <strong>Tuesday {formatDate(weekDates[1])} at 2:00 PM EST</strong> · Topic: {videoTopic}
                </div>
              </div>
            )}
          </>
        )}

        {/* Footer */}
        <div style={{
          textAlign: "center", marginTop: "48px", paddingTop: "24px",
          borderTop: "1px solid rgba(255,255,255,0.04)",
          fontSize: "9px", letterSpacing: "3px", color: "#1e293b", textTransform: "uppercase",
        }}>
          Shadows in the Machine · Production System · Built for Manish
        </div>
      </div>
    </div>
  );
}

function hexToRgb(hex) {
  const r = parseInt(hex.slice(1, 3), 16);
  const g = parseInt(hex.slice(3, 5), 16);
  const b = parseInt(hex.slice(5, 7), 16);
  return `${r},${g},${b}`;
}
