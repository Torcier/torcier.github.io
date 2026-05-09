<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Workout Questionnaire</title>
 <style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    }

    html { 
        overflow-y: scroll; 
    }

    body {
        background: #08090A;
        color: #e0e0e0;
        min-height: 100vh;
        padding: 15px 0;
        display: flex;
        justify-content: center;
        align-items: flex-start;
    }

    .container {
        width: 100%;
        max-width: 800px;
    }

   .plan-section {
    width: 100%;
    max-width: 800px;
    background: transparent;
    padding: 25px 20px;
    min-height: 100vh;
    position: absolute;
    left: 0;
    right: 0;
    margin: 0 auto;
    
    opacity: 0;
    transition: opacity 0.35s ease;
    pointer-events: none;
}

.plan-section.active {
    opacity: 1;
    pointer-events: auto;
    z-index: 10;
}

/* Staggered Animation */
.plan-section .question,
.plan-section .choices,
.plan-section .navigation {
    opacity: 0;
    transform: translateY(25px);
    transition: all 0.45s cubic-bezier(0.4, 0, 0.2, 1);
}

.plan-section.active .question { 
    opacity: 1; 
    transform: translateY(0); 
    transition-delay: 60ms; 
}

.plan-section.active .choices { 
    opacity: 1; 
    transform: translateY(0); 
    transition-delay: 160ms; 
}

.plan-section.active .navigation { 
    opacity: 1; 
    transform: translateY(0); 
    transition-delay: 300ms; 
}

/* Exit animation - slides up */
.plan-section.exiting {
    opacity: 0;
    transform: translateY(-80px);
}
    .question {
        font-size: 1.8rem;
        margin-bottom: 30px;
        text-align: center;
        font-weight: 600;
        color: #FAFAFA;
    }

    .choices {
        display: grid;
        gap: 15px;
        margin-bottom: 30px;
        min-height: 160px;
        align-content: center;
    }

    .choice {
        background: rgba(255,255,255,0.08);
        padding: 15px;
        border-radius: 12px;
        text-align: center;
        cursor: pointer;
        transition: all 0.3s ease;
        border: 1px solid rgba(255,255,255,0.1);
        font-size: 1.1rem;
        position: relative;
        user-select: none;
    }

/* ==================== PRESS ANIMATION ==================== */
.nav-button,
.choice,
.day-square,
.stage-button,
.info-button,
.sets-rest-value,
.exercise-header {
    position: relative;
    transition: transform 0.25s ease, 
                filter 0.25s ease,
                background-color 0.25s ease;
    user-select: none;
    -webkit-tap-highlight-color: transparent;
    touch-action: manipulation;
    outline: none;
}

    .navigation {
        display: flex;
        justify-content: space-between;
        margin-top: 30px;
        padding-top: 20px;
    }

    .nav-button {
    background: rgba(255,255,255,0.1);
    color: #FAFAFA;               /* ← This makes the text white */
    padding: 10px 20px;
    border-radius: 10px;
    cursor: pointer;
    transition: all 0.3s ease;
    border: 1px solid rgba(255,255,255,0.1);
    font-size: 1rem;
}

    .nav-button:disabled {
    background: rgba(255,255,255,0.05);
    color: #ffffff;
    cursor: not-allowed;
    opacity: 0.5;
}

    .plan-section h2 { 
        font-size: 2rem; 
        margin-bottom: 25px; 
        text-align: center; 
    }

    .day-selector {
        background: rgba(255,255,255,0.08);
        border-radius: 50px;
        padding: 7px;
        display: flex;
        gap: 6px;
        margin-bottom: 20px;
        overflow-x: auto;
        scrollbar-width: none;
    }

    .day-selector::-webkit-scrollbar { 
        display: none; 
    }
/* ==================== DAY SQUARES ==================== */
.day-square {
    flex: 1 1 auto;
    padding: 13px 12px;
    border-radius: 40px;
    font-weight: 600;
    font-size: 1.02rem;
    text-align: center;
    cursor: pointer;
    transition: all 0.25s ease;
    white-space: nowrap;
    min-width: 68px;
    color: #ffffff;
}

/* Press effect when tapping/holding */
.day-square:active {
    background: rgba(255,255,255,0.25) !important;
    transform: scale(0.94);
    filter: brightness(1.3);
}

/* Selected Active State */
.day-square.active {
    background: #FAFAFA !important;
    color: #1a1a2e !important;
    box-shadow: 0 4px 12px rgba(0,0,0,0.25) !important;
    transform: none !important;
    filter: none !important;
}

    .stage-selector {
        background: rgba(255,255,255,0.08);
        border-radius: 30px;
        padding: 6px 10px;
        display: inline-flex;
        gap: 6px;
        margin-bottom: 25px;
        overflow-x: auto;
        scrollbar-width: none;
    }

    .stage-selector::-webkit-scrollbar { 
        display: none; 
    }

    .stage-button {
        padding: 8px 16px;
        border-radius: 30px;
        font-weight: 600;
        font-size: 0.92rem;
        text-align: center;
        cursor: pointer;
        transition: all 0.3s ease;
        white-space: nowrap;
        min-width: 74px;
        background: transparent;
        color: #e0e0e0;
        flex-shrink: 0;
    }

    .stage-button.recommended {
        background: #ffffff;
        color: #1a1a2e;
        box-shadow: 0 3px 10px rgba(0,0,0,0.2);
    }

    .exercise-card {
    background: rgba(255,255,255,0.07);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 16px;
    overflow: hidden;
    margin-bottom: 14px;
    transition: all 0.3s ease;
    position: relative;
    
    /* Extra smoothness */
    will-change: transform, opacity;
}

    .exercise-header {
        padding: 14px 20px;
        cursor: pointer;
        display: flex;
        flex-direction: column;
        gap: 4px;
    }

    .exercise-name {
        font-size: 0.89rem;
        font-weight: 600;
        line-height: 1.3;
    }

    .sets-rest {
        margin: 0 20px 12px 20px;
        font-size: 0.87rem;
        display: flex;
        align-items: center;
        gap: 10px;
        flex-wrap: wrap;
    }

    .sets-rest .label { 
        opacity: 0.75; 
        font-size: 0.82rem; 
    }

    .sets-rest-value {
        background: rgba(255,255,255,0.1);
        padding: 4px 10px;
        border-radius: 6px;
        cursor: pointer;
        transition: all 0.2s ease;
        min-width: 44px;
        text-align: center;
        border: 1px solid rgba(255,255,255,0.15);
        font-weight: 500;
        display: inline-flex;
        align-items: center;
        gap: 3px;
        font-size: 0.86rem;
    }

    .exercise-video-container { 
        padding: 0 20px 12px 20px; 
    }

    .exercise-video-container video {
        width: 100%;
        max-height: 380px;
        border-radius: 12px;
        background: #111;
        display: block;
    }

    .alternatives-list {
        margin: 0 20px 16px 20px;
        background: rgba(255,255,255,0.07);
        border-radius: 12px;
        overflow: hidden;
        max-height: 0;
        opacity: 0;
        padding: 0 12px;
        transition: max-height 0.42s cubic-bezier(0.4, 0, 0.2, 1),
                    opacity 0.30s ease,
                    padding 0.42s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .alternatives-list.open {
        max-height: 620px;
        opacity: 1;
        padding: 12px;
    }

    .alternatives-list div {
        padding: 10px 14px;
        cursor: pointer;
        border-radius: 8px;
        color: #e0e0ff;
        transition: background 0.2s;
        font-size: 0.87rem;
    }

    .info-buttons {
        display: flex;
        justify-content: flex-start;
        gap: 10px;
        margin: 25px 0 20px;
        flex-wrap: wrap;
    }

    .info-button {
        background: rgba(255,255,255,0.08);
        padding: 9px 18px;
        border-radius: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
        border: 1px solid rgba(255,255,255,0.1);
        font-size: 0.95rem;
    }

    .info-button.active-info {
        background: rgba(76,175,80,0.18);
        border-color: #FAFAFA;
    }

    .info-content-container {
        display: none;
        background: rgba(255,255,255,0.05);
        border: 1px solid rgba(255,255,255,0.1);
        border-radius: 12px;
        margin: 15px 0 30px;
        overflow: hidden;
        transition: all 0.4s ease-in-out;
    }

    .info-content-container.visible {
        display: block;
        padding: 20px;
        background: rgba(255,255,255,0.07);
    }

    /* Press Animation */
.nav-button:active,
.choice:active,
.stage-button:active,
.info-button:active,
.sets-rest-value:active,
.exercise-header:active {
    transform: scale(0.94);
    filter: brightness(1.3);
    background-color: rgba(255, 255, 255, 0.22) !important;
}

.nav-button.active,
.choice.active,
.stage-button.active,
.info-button.active,
.sets-rest-value.active,
.exercise-header.active {
    transform: scale(0.95);
    filter: brightness(0.82);
}
/* ====================== EXERCISE CARD ANIMATION ====================== */
.exercise-card {
    background: rgba(255,255,255,0.07);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 16px;
    overflow: hidden;
    margin-bottom: 14px;
    transition: all 0.3s ease;
    position: relative;
    will-change: transform, opacity;
}

/* Animation for NEWLY added exercises only */
.exercise-card.new {
    opacity: 0;
    transform: translateY(40px);
    transition: all 0.55s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.exercise-card.new.visible {
    opacity: 1;
    transform: translateY(0);
}
/* Fullscreen / Maximize Button */
.fullscreen-button {
    position: fixed;
    top: 15px;
    right: 15px;
    z-index: 10000;
    width: 48px;
    height: 48px;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.15);
    color: #e0e0e0;
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    cursor: pointer;
    transition: all 0.3s ease;
    backdrop-filter: blur(8px);
    user-select: none;
}

.fullscreen-button:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: scale(1.05);
}

.fullscreen-button:active {
    transform: scale(0.92);
}
// ====================== BEST MAXIMIZE FOR WIX ======================
document.addEventListener('DOMContentLoaded', function() {
    const btn = document.getElementById('fullscreen-btn');
    if (!btn) return;

    let isExpanded = false;

    btn.addEventListener('click', function() {
        isExpanded = !isExpanded;

        if (isExpanded) {
            btn.innerHTML = '❐';
            btn.title = "Restore Normal View";

            // Expand everything
            document.querySelectorAll('.container').forEach(c => {
                c.style.maxWidth = '100%';
                c.style.width = '100%';
                c.style.margin = '0';
                c.style.padding = '0 10px';
            });

            document.body.style.margin = '0';
            document.body.style.padding = '0';
            document.documentElement.style.height = '100vh';
            document.body.style.height = '100vh';
            document.body.style.overflow = 'hidden';

            // Aggressive attempt to hide Wix header/footer
            const hideStyle = document.createElement('style');
            hideStyle.innerHTML = `
                header, footer, #siteHeader, .wix-header, .wix-footer, 
                [data-testid="header"], [data-testid="footer"] { 
                    display: none !important; 
                }
            `;
            document.head.appendChild(hideStyle);

        } else {
            btn.innerHTML = '⛶';
            btn.title = "Maximize";

            // Restore
            document.querySelectorAll('.container').forEach(c => {
                c.style.maxWidth = '800px';
                c.style.width = '100%';
                c.style.padding = '';
                c.style.margin = '';
            });
            document.body.style.overflow = '';
        }
    });

    console.log("Maximize button ready");
});
</style>
</head>
<body>
    <!-- Fullscreen Button -->
    <button id="fullscreen-btn" class="fullscreen-button" title="Toggle Fullscreen">
        ⛶
    </button>

    <div class="container">
        <div class="plan-section" id="page1">
    <div class="question">How many sessions a week do you want?</div>
<div class="choices" id="sessions-choices">
    <div class="choice" data-sessions="1">1</div>
    <div class="choice" data-sessions="2">2</div>
    <div class="choice" data-sessions="3">3</div>
    <div class="choice" data-sessions="4">4</div>
    <div class="choice" data-sessions="5">5</div>
    <div class="choice" data-sessions="6">6</div>
    <div class="choice" data-sessions="7">7</div>
</div>
    <div class="navigation">
        <button class="nav-button" disabled>Back</button>
    </div>
</div>

                <div class="plan-section" id="page2" style="display:none;">
            <div class="question">How long do you want the sessions to be?</div>
            <div class="choices" id="duration-choices">
                <div class="choice" data-next="page3">20-25 minutes</div>
                <div class="choice" data-next="page3">25-35 minutes</div>
                <div class="choice" data-next="page3">35-45 minutes</div>
            </div>
            <div class="navigation">
                <button class="nav-button">Back</button>
            </div>
        </div>

        <div class="plan-section" id="page3" style="display:none;">
    <div class="question">Entire body sessions or split?</div>
    <div class="choices" id="split-choices"></div>
    <div class="navigation">
        <button class="nav-button">Back</button>
    </div>
</div>

                <div class="plan-section" id="page4" style="display:none;">
            <div class="question">Pain somewhere?</div>
            <div class="choices" id="pain-choices">
                <div class="choice" data-next="page5">Knees</div>
                <div class="choice" data-next="page5">Elbows</div>
                <div class="choice" data-next="page5">Shoulders</div>
            </div>
            <div class="navigation">
                <button class="nav-button">Back</button>
                
            </div>
        </div>

                <div class="plan-section" id="page5" style="display:none;">
            <div class="question">Ramp up period?</div>
            <div class="choices" id="ramp-choices">
                <div class="choice" data-next="page6">1-3 Weeks</div>
                <div class="choice" data-next="page6">3-5 Weeks</div>
                <div class="choice" data-next="page6">5-8 Weeks</div>
            </div>
            <div class="navigation">
                <button class="nav-button">Back</button>
                
            </div>
        </div>

        <div class="plan-section" id="page6" style="display:none;">
    <h2>Your Plan</h2>
    <div class="day-selector" id="day-selector"></div>
    <div class="stage-selector" id="stage-selector"></div>
    <div class="plan-items" id="plan-items"></div>
    
    <div class="info-buttons" id="info-buttons">
    <div class="info-button" data-tab="reasoning">Reasoning</div>
    <div class="info-button" data-tab="strategy">Strategy</div>
</div>
    <div class="info-content-container" id="info-content"></div>
    
    <div class="navigation">
        <button class="nav-button">Back</button>
        <button class="nav-button" disabled>Next</button>
    </div>
</div>
    </div>

    <script>
       const appState = {
    sessionsPerWeek: 3,
    selectedSplit: null,
    currentStage: 1,
    currentInfoTab: null,
    lastOpenKey: null,
    customPlan: {}
};

        const workoutPlans = {
            "Push-Pull-Legs": {
                push: [
                    { name: "Dumbbell Incline Press", sets: 3, rest: "1-3 min", stage: 1, alternatives: ["Barbell Flat Bench Press", "Barbell Incline Bench Press", "Seated Chest Press Machine", "Dumbbell Flat Bench Press"] },
                    { name: "Seated Chest Press Machine", sets: 2, rest: "1-3 min", stage: 1, alternatives: ["Barbell Flat Bench Press", "Barbell Incline Bench Press", "Weighted Dips (Chest-Focused)", "Dumbbell Flat Bench Press"] },
                    { name: "Cable Fly Low", sets: 1, rest: "1 min", stage: 1, alternatives: ["Pec Deck Machine (Butterfly)"] },
                    { name: "Tricep Pushdowns", sets: 2, rest: "1 min", stage: 1, alternatives: ["Rope Overhead Cable Extension", "One-Arm Cable Triceps Extension"] },
                    { name: "Dumbbell Shoulder Press (Seated or Standing)", sets: 2, rest: "1-2 min", stage: 2, alternatives: ["Machine Shoulder Press", "Smith Machine Overhead Press", "Barbell Overhead Press (Seated or Standing)"] },
                    { name: "Cable Lateral Raise Single arm", sets: 4, rest: "1 min", stage: 3, alternatives: ["Dumbbell Lateral Raise (standing or seated)", "Machine Lateral Raise"] }
                ],
                pull: [
                    { name: "Lat Pulldown (Cable)", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Close-Grip Pulldown (Cable)", "Pull-Ups", "Assisted Pull-Ups"] },
                    { name: "Seated Cable Row", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Barbell Bent-Over Row", "Machine Row (Chest Supported)"] },
                    { name: "EZ Bar Curl (Standing)", sets: 2, rest: "1 min", stage: 1, alternatives: ["Concentration Curl", "Cable One-Arm Curl", "Hammer Curl (Dumbbell)", "Cable Curl (Straight/EZ/Rope Attachment)"] },
                    { name: "Rear Delt Machine Fly", sets: 3, rest: "1 min", stage: 2, alternatives: ["Cable Reverse Fly (single-arm optional)", "Face Pulls (cable)", "Rear Delt Dumbbell Fly (reverse fly, bent-over)"] },
                    { name: "Dumbbell One-Arm Row", sets: 2, rest: "2-3 min", stage: 3, alternatives: ["Machine Row (Chest Supported)", "T-Bar Row (Barbell or Machine)"] }
                ],
                legs: [
                    { name: "Leg Press (Machine)", sets: 2, rest: "3 min", stage: 1, alternatives: ["Barbell Back Squat", "Smith Machine Squat"] },
                    { name: "Romanian Deadlift (Barbell or Dumbbell)", sets: 2, rest: "1-3 min", stage: 1, alternatives: ["Bulgarian Split Squat (Dumbbells or Barbell)", "Hip Thrust (Barbell or Machine)"] },
                    { name: "Leg Extension (Machine)", sets: "2-3", rest: "1-2 min", stage: 1, alternatives: ["Belt Squat (Machine or Lever Type)", "Dumbbell Walking Lunges"] },
                    { name: "Leg Curl (Lying or Seated Machine)", sets: 1, rest: "1-2 min", stage: 2, alternatives: ["Cable Kickbacks", "Glute Kickbacks"] },
                    { name: "Machine Crunch (Seated or Lying)", sets: 2, rest: "1 min", stage: 3, alternatives: ["Cable Crunch (High Pulley)", "Hanging Knee Raise (Leg Raise)"] },
                    { name: "Lower Back Extension Machine", sets: 2, rest: "1 min", stage: 3, alternatives: ["Good Morning (Barbell or Smith Machine)"] }
                ]
            },
            "Entire body": {
                exercises: [
                    { name: "Flat Bench Press", sets: 2, rest: "1-3 min", stage: 1, alternatives: ["Dumbbell Incline Press", "Seated Chest Press Machine"] },
                    { name: "Close-Grip Barbell Bench Press", sets: 2, rest: "1-3 min", stage: 2, alternatives: ["Weighted Dips (Chest-Focused)"] },
                    { name: "Leg Press (Machine)", sets: 2, rest: "3 min", stage: 1, alternatives: ["Barbell Back Squat", "Smith Machine Squat"] },
                    { name: "Dumbbell Walking Lunges", sets: 2, rest: "2 min", stage: 2, alternatives: ["Belt Squat (Machine or Lever Type)", "Hack Squat Machine"] },
                    { name: "Lat Pulldown (Cable)", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Close-Grip Pulldown (Cable)", "Pull-Ups", "Assisted Pull-Ups"] },
                    { name: "Seated Cable Row", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Barbell Bent-Over Row", "Machine Row"] },
                    { name: "Face Pulls (cable)", sets: 2, rest: "1 min", stage: 2, alternatives: ["Rear Delt Machine Fly", "Rear Delt Dumbbell Fly (reverse fly, bent-over)"] },
                    { name: "Cable Curl", sets: 1, rest: "1 min", stage: 3, alternatives: ["Concentration Curl", "EZ Bar Curl (Standing)"] },
                    { name: "Cable Lateral Raise Single arm", sets: 2, rest: "1 min", stage: 3, alternatives: ["Dumbbell Lateral Raise (standing or seated)", "Machine Lateral Raise"] }
                ]
            },
            "Entire Body 3": {
                session1: [
                    { name: "Flat Bench Press", sets: 2, rest: "1-3 min", stage: 2, alternatives: ["Dumbbell Incline Press", "Seated Chest Press Machine"] },
                    { name: "Close-Grip Barbell Bench Press", sets: 2, rest: "1-3 min", stage: 2, alternatives: ["Weighted Dips (Chest-Focused)"] },
                    { name: "Leg Press (Machine)", sets: 2, rest: "3 min", stage: 1, alternatives: ["Barbell Back Squat", "Smith Machine Squat"] },
                    { name: "Leg Extension (Machine)", sets: 2, rest: "1-2 min", stage: 1, alternatives: ["Dumbbell Walking Lunges", "Belt Squat"] },
                    { name: "Lat Pulldown (Cable)", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Close-Grip Pulldown (Cable)", "Pull-Ups", "Assisted Pull-Ups"] },
                    { name: "Cable Lateral Raise Single arm", sets: 2, rest: "1 min", stage: 3, alternatives: ["Dumbbell Lateral Raise (standing or seated)", "Machine Lateral Raise"] }
                ],
                session2: [
                    { name: "Flat Bench Press", sets: 2, rest: "1-3 min", stage: 2, alternatives: ["Dumbbell Incline Press", "Seated Chest Press Machine"] },
                    { name: "Close-Grip Barbell Bench Press", sets: 2, rest: "1-3 min", stage: 2, alternatives: ["Weighted Dips (Chest-Focused)"] },
                    { name: "Romanian Deadlift (Barbell or Dumbbell)", sets: 2, rest: "1-3 min", stage: 1, alternatives: ["Bulgarian Split Squat (Dumbbells or Barbell)", "Hip Thrust (Barbell or Machine)"] },
                    { name: "Leg Curl (Lying or Seated Machine)", sets: 2, rest: "1 min", stage: 3, alternatives: [] },
                    { name: "Seated Cable Row", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Barbell Bent-Over Row", "Machine Row (Chest Supported)"] },
                    { name: "Machine Crunch (Seated or Lying)", sets: "2-4", rest: "1 min", stage: 1, alternatives: ["Cable Crunch (High Pulley)", "Hanging Knee Raise (Leg Raise)"] },
                    { name: "Optional: Standing Calf Raise (Machine or Free Weight)", sets: "2-5", rest: "1 min", stage: 3, alternatives: ["Any Calf Exercise (prefer standing/stretched variation)"] }
                ],
                session3: [
                    { name: "Seated Chest Press Machine", sets: 2, rest: "1-3 min", stage: 1, alternatives: ["Incline Bench Press", "Incline Plate-Loaded Chest Press Machine"] },
                    { name: "Leg Press (Machine)", sets: 2, rest: "3 min", stage: 1, alternatives: ["Barbell Back Squat", "Smith Machine Squat"] },
                    { name: "Seated Cable Row", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Barbell Bent-Over Row", "Machine Row (Chest Supported)"] },
                    { name: "Lat Pulldown (Cable)", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Close-Grip Pulldown (Cable)", "Pull-Ups", "Assisted Pull-Ups"] },
                    { name: "Cable Lateral Raise Single arm", sets: "2-3", rest: "1 min", stage: 2, alternatives: ["Dumbbell Lateral Raise (standing or seated)", "Machine Lateral Raise"] },
                    { name: "Face Pulls (cable)", sets: "2-4", rest: "1 min", stage: 3, alternatives: ["Rear Delt Machine Fly", "Rear Delt Dumbbell Fly (reverse fly, bent-over)"] }
                ]
            },
            "Upper/Lower": {
                upper: [
                    { name: "Dumbbell Incline Press", sets: 2, rest: "1-3 min", stage: 1, alternatives: ["Flat Bench Press", "Smith Machine Bench Press (flat/incline)"] },
                    { name: "Close-Grip Barbell Bench Press", sets: 2, rest: "1-2 min", stage: 3, alternatives: ["Weighted Dips (Chest-Focused)"] },
                    { name: "Seated Chest Press Machine", sets: 1, rest: "1-3 min", stage: 1, alternatives: ["Incline Bench Press", "Incline Plate-Loaded Chest Press Machine"] },
                    { name: "Lat Pulldown (Cable)", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Close-Grip Pulldown (Cable)", "Pull-Ups", "Assisted Pull-Ups"] },
                    { name: "Seated Cable Row", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Barbell Bent-Over Row", "Machine Row (Chest Supported)"] },
                    { name: "Face Pulls (cable)", sets: 2, rest: "1 min", stage: 3, alternatives: ["Rear Delt Machine Fly", "Rear Delt Dumbbell Fly (reverse fly, bent-over)"] },
                    { name: "Cable Curl (Straight/EZ/Rope Attachment)", sets: 1, rest: "1 min", stage: 2, alternatives: ["Concentration Curl", "EZ Bar Curl (Standing)"] },
                    { name: "Cable Lateral Raise Single arm", sets: 2, rest: "1 min", stage: 2, alternatives: ["Dumbbell Lateral Raise (standing or seated)", "Machine Lateral Raise"] }
                ],
                lower: [
                    { name: "Leg Press (Machine)", sets: 2, rest: "3 min", stage: 1, alternatives: ["Barbell Back Squat", "Smith Machine Squat"] },
                    { name: "Romanian Deadlift (Barbell or Dumbbell)", sets: 2, rest: "1-3 min", stage: 1, alternatives: ["Bulgarian Split Squat", "Hip Thrust"] },
                    { name: "Leg Extension (Machine)", sets: "2-3", rest: "1-2 min", stage: 1, alternatives: ["Dumbbell Walking Lunges", "Belt Squat"] },
                    { name: "Leg Curl (Lying or Seated Machine)", sets: 1, rest: "1-2 min", stage: 2, alternatives: ["Cable Kickbacks", "Glute Kickbacks"] },
                    { name: "Machine Crunch (Seated or Lying)", sets: 2, rest: "1 min", stage: 3, alternatives: ["Cable Crunch", "Hanging Knee Raise"] }
                ]
            },
            "Upper/Lower 4": {
                upper: [
                    { name: "Dumbbell Incline Press", sets: 2, rest: "1-3 min", stage: 1, alternatives: ["Flat Bench Press", "Smith Machine Bench Press"] },
                    { name: "Seated Chest Press Machine", sets: 1, rest: "1-3 min", stage: 1, alternatives: ["Incline Bench Press"] },
                    { name: "Lat Pulldown (Cable)", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Pull-Ups"] },
                    { name: "Seated Cable Row", sets: 2, rest: "2-3 min", stage: 1, alternatives: ["Barbell Bent-Over Row"] },
                    { name: "Cable Lateral Raise Single arm", sets: 2, rest: "1 min", stage: 2, alternatives: ["Dumbbell Lateral Raise"] }
                ],
                lower: [
                    { name: "Leg Press (Machine)", sets: 3, rest: "3 min", stage: 1, alternatives: ["Barbell Back Squat"] },
                    { name: "Romanian Deadlift", sets: 2, rest: "2 min", stage: 1, alternatives: ["Hip Thrust"] },
                    { name: "Leg Extension", sets: 2, rest: "1 min", stage: 1, alternatives: [] },
                    { name: "Leg Curl", sets: 2, rest: "1 min", stage: 2, alternatives: [] }
                ],
                upper2: [
                    { name: "Close-Grip Barbell Bench Press", sets: 2, rest: "1-2 min", stage: 2, alternatives: ["Weighted Dips"] },
                    { name: "Face Pulls (cable)", sets: 3, rest: "1 min", stage: 2, alternatives: ["Rear Delt Machine Fly"] },
                    { name: "Cable Curl", sets: 2, rest: "1 min", stage: 2, alternatives: ["EZ Bar Curl"] }
                ],
                lower2: [
                    { name: "Bulgarian Split Squat", sets: 2, rest: "2 min", stage: 2, alternatives: [] },
                    { name: "Machine Crunch", sets: 3, rest: "1 min", stage: 3, alternatives: [] }
                ]
            }
        };

        const videoMap = {
            "EZ Bar Curl (Standing)": "https://video.wixstatic.com/video/f511b1_3553cbed0aa944bf9c59d61a858dc910/1080p/mp4/file.mp4",
            "Concentration Curl": "https://video.wixstatic.com/video/f511b1_0213bbea2c6e47d8b0a9088f282536f4/720p/mp4/file.mp4"
        };
// ==================== CLEAN EVENT LISTENERS ====================function // ==================== CLEAN EVENT LISTENERS ====================
function attachAllListeners() {

    // PAGE 1: Sessions per week
document.getElementById('sessions-choices')?.addEventListener('click', (e) => {
    const choice = e.target.closest('.choice');
    if (!choice || !choice.dataset.sessions) return;

    appState.sessionsPerWeek = parseInt(choice.dataset.sessions);
    
    // Removed the green styling block entirely
    navigateWithDelay('page2');
});

    // PAGE 2, 4, 5: Simple next-page choices
    ['duration-choices', 'pain-choices', 'ramp-choices'].forEach(id => {
        document.getElementById(id)?.addEventListener('click', (e) => {
            const choice = e.target.closest('.choice');
            if (choice?.dataset.next) {
                navigateWithDelay(choice.dataset.next);   // Important
            }
        });
    });

    // PAGE 3: Split selection
    document.getElementById('split-choices')?.addEventListener('click', (e) => {
        const choice = e.target.closest('.choice');
        if (!choice) return;

        appState.selectedSplit = choice.textContent.trim();
        navigateWithDelay('page4');   // Important
    });

    // === BACK BUTTONS ===
    document.querySelector('#page2 .nav-button:first-child')?.addEventListener('click', () => {
        navigateWithDelay('page1');
    });

    document.querySelector('#page3 .nav-button:first-child')?.addEventListener('click', () => {
        navigateWithDelay('page2');
    });

    document.querySelector('#page4 .nav-button:first-child')?.addEventListener('click', () => {
        navigateWithDelay('page3');
    });

    document.querySelector('#page5 .nav-button:first-child')?.addEventListener('click', () => {
        navigateWithDelay('page4');
    });

    document.querySelector('#page6 .nav-button:first-child')?.addEventListener('click', () => {
        navigateWithDelay('page5');
    });

    // INFO BUTTONS (no navigation)
    const infoContainer = document.getElementById('info-buttons');
    if (infoContainer) {
        infoContainer.addEventListener('click', function(e) {
            const btn = e.target.closest('.info-button');
            if (!btn) return;
            const tab = btn.dataset.tab;
            appState.currentInfoTab = (appState.currentInfoTab === tab) ? null : tab;

            document.querySelectorAll('.info-button').forEach(b => {
                b.classList.toggle('active-info', b.dataset.tab === appState.currentInfoTab);
            });

            renderInfoContent();
            updateLayout();
        });
    }
}
        

        function showVideoForExercise(wrapper, exerciseName) {
            const videoCont = wrapper.querySelector('.exercise-video-container');
            if (!videoCont) return;
            videoCont.innerHTML = `
                <video controls loop muted playsinline>
                    <source src="${videoMap[exerciseName] || ''}" type="video/mp4">
                </video>
            `;
            videoCont.style.display = videoMap[exerciseName] ? 'block' : 'none';
            const vid = videoCont.querySelector('video');
            if (vid) vid.play().catch(() => {});
        }

        function closeAlternativesList(list) {
            if (!list) return;
            list.style.opacity = '0';
            setTimeout(() => {
                list.classList.remove('open');
                list.style.maxHeight = '0';
                list.style.padding = '0 12px';
            }, 20);
            setTimeout(() => {
                list.style.opacity = '';
                list.style.maxHeight = '';
                list.style.padding = '';
            }, 450);
        }

        function setStage(stage) {
    appState.currentStage = stage;
    renderStages();
    
    const active = document.querySelector('.day-square.active');
    if (active) {
        switchDay(parseInt(active.dataset.day));
    } else {
        switchDay(1);
    }
    
    renderInfoContent();
    updateLayout();
}

        function renderInfoContent() {
    const container = document.getElementById('info-content');
    if (!container) return;

    if (!appState.currentInfoTab) {
        container.classList.remove('visible');
        container.innerHTML = '';
        return;
    }

    container.classList.add('visible');

    container.innerHTML =
        appState.currentInfoTab === 'reasoning'
            ? `<p>Your plan is built based on frequency, session length, split type, pain considerations, and ramp-up period.</p>
               <p>Stages gradually increase volume and complexity.</p>`
            : `<p>Focus on form first — especially during stage 1.</p>
               <p>Use progressive overload: add weight/reps when comfortable.</p>
               <p>Prioritize recovery: sleep, protein, listen to your body.</p>`;
}
// ==================== NEW LAYOUT FUNCTION ====================
function updateLayout() {
    const page6 = document.getElementById('page6');
    if (page6 && page6.style.display !== 'none') {
        window.scrollTo({ top: 0, behavior: 'instant' });
    }
}

                        function renderStages() {
            const container = document.getElementById('stage-selector');
            container.innerHTML = '';
            for (let i = 1; i <= 3; i++) {
                const el = document.createElement('div');
                el.className = `stage-button ${i === appState.currentStage ? 'recommended' : ''}`;
                el.textContent = `STAGE ${i}`;
                el.addEventListener('click', () => setStage(i));
                container.appendChild(el);
            }
        }

        function makeEditable(valueEl, key, type) {
    const currentText = valueEl.textContent.trim().replace(/ min$/, '');

    const input = document.createElement('input');
    input.type = 'text';
    input.value = currentText;
    input.style.width = '100%';
    input.style.textAlign = 'center';

    valueEl.innerHTML = '';
    valueEl.appendChild(input);

    input.focus();
    input.select();

    function saveEdit() {
        let newValue = input.value.trim();
        if (!newValue) newValue = currentText;

        // ✅ CORRECT customPlan fix (this is your #7)
        if (!appState.customPlan[key]) {
            appState.customPlan[key] = {};
        }

        appState.customPlan[key][type] =
            type === 'rest' ? newValue + ' min' : newValue;

        const activeDayEl = document.querySelector('.day-square.active');
        if (activeDayEl) {
            switchDay(parseInt(activeDayEl.dataset.day));
        }
    }

    input.addEventListener('blur', saveEdit);
    input.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') input.blur();
    });
}

        function switchDay(day) {
    if (day > appState.sessionsPerWeek) return;

    // Update active day
    document.querySelectorAll('.day-square').forEach(el => el.classList.remove('active'));
    const activeDay = document.querySelector(`.day-square[data-day="${day}"]`);
    if (activeDay) activeDay.classList.add('active');

    if (!appState.selectedSplit || !workoutPlans[appState.selectedSplit]) {
        document.getElementById('plan-items').innerHTML = '<p style="text-align:center;opacity:0.7;">Select a split to view plan</p>';
        updateLayout();
        return;
    }

    const plan = workoutPlans[appState.selectedSplit];
    let exs = [];

    // Get exercises for this day
    if (appState.selectedSplit === "Push-Pull-Legs") {
        if (day === 1) exs = plan.push || [];
        else if (day === 2) exs = plan.pull || [];
        else if (day === 3) exs = plan.legs || [];
    } else if (appState.selectedSplit === "Entire body") {
        exs = plan.exercises || [];
    } else if (appState.selectedSplit === "Entire Body 3") {
        if (day === 1) exs = plan.session1 || [];
        else if (day === 2) exs = plan.session2 || [];
        else if (day === 3) exs = plan.session3 || [];
    } else if (appState.selectedSplit === "Upper/Lower") {
        const isUpper = (day % 2 === 1);
        exs = isUpper ? (plan.upper || []) : (plan.lower || []);
    } else if (appState.selectedSplit === "Upper/Lower 4") {
        let cycle = (day - 1) % 4;
        if (cycle === 0) exs = plan.upper || [];
        else if (cycle === 1) exs = plan.lower || [];
        else if (cycle === 2) exs = plan.upper2 || [];
        else exs = plan.lower2 || [];
    }

    const visibleExercises = exs.filter(e => (e.stage || 1) <= appState.currentStage);
    const itemsContainer = document.getElementById('plan-items');

    // Create a map of currently visible exercise keys
    const currentKeys = new Set();
    visibleExercises.forEach(ex => {
        currentKeys.add(`${day}-${ex.name}`);
    });

    // 1. Remove exercises that are no longer visible (optional fade out)
    Array.from(itemsContainer.children).forEach(card => {
        const key = card.dataset.key;
        if (!key || !currentKeys.has(key)) {
            card.style.transition = 'all 0.3s ease';
            card.style.opacity = '0';
            card.style.transform = 'translateY(-20px)';
            setTimeout(() => card.remove(), 300);
        }
    });

    // 2. Add or update exercises
    visibleExercises.forEach((ex, index) => {
        const key = `${day}-${ex.name}`;
        let wrapper = itemsContainer.querySelector(`.exercise-card[data-key="${key}"]`);

        // If it already exists → just make sure it's visible (no animation)
        if (wrapper) {
            wrapper.style.opacity = '1';
            wrapper.style.transform = 'none';
            return;
        }

        // === CREATE NEW CARD ===
        wrapper = document.createElement('div');
        wrapper.className = 'exercise-card new';
        wrapper.dataset.key = key;

        const saved = appState.customPlan[key] || {};
        const currentName = saved.name || ex.name;
        const currentSets = saved.sets !== undefined ? saved.sets : ex.sets;
        const currentRest = saved.rest !== undefined ? saved.rest : ex.rest;

        // Build the full card (same as before)
        const header = document.createElement('div');
        header.className = 'exercise-header';

        const nameEl = document.createElement('div');
        nameEl.className = 'exercise-name';
        nameEl.textContent = currentName;
        header.appendChild(nameEl);

        const videoCont = document.createElement('div');
        videoCont.className = 'exercise-video-container';
        videoCont.style.display = 'none';

        const altList = document.createElement('div');
        altList.className = 'alternatives-list';

        if (ex.alternatives && ex.alternatives.length > 0) {
            let alts = [ex.name, ...ex.alternatives.filter(a => a !== ex.name)];
            alts.forEach(alt => {
                const altItem = document.createElement('div');
                altItem.textContent = alt;
                altItem.onclick = (e) => {
                    e.stopPropagation();
                    if (!appState.customPlan[key]) appState.customPlan[key] = {};
                    appState.customPlan[key].name = (alt === ex.name) ? null : alt;
                    nameEl.textContent = alt;
                    showVideoForExercise(wrapper, alt);
                    updateLayout();
                };
                altList.appendChild(altItem);
            });
        }

        const setsRest = document.createElement('div');
        setsRest.className = 'sets-rest';
        setsRest.innerHTML = `
            <span class="label">Sets</span>
            <span class="sets-rest-value" data-type="sets">${currentSets}</span>
            <span class="label">Rest</span>
            <span class="sets-rest-value" data-type="rest">${currentRest}</span>
        `;

        setsRest.querySelectorAll('.sets-rest-value').forEach(valueEl => {
            valueEl.addEventListener('click', (e) => {
                e.stopPropagation();
                makeEditable(valueEl, key, valueEl.dataset.type);
            });
        });

        header.onclick = () => {
            const isOpen = altList.classList.contains('open');
            document.querySelectorAll('.alternatives-list.open').forEach(l => {
                if (l !== altList) closeAlternativesList(l);
            });
            document.querySelectorAll('.exercise-video-container').forEach(v => v.style.display = 'none');

            if (isOpen) {
                closeAlternativesList(altList);
                appState.lastOpenKey = null;
            } else {
                altList.classList.add('open');
                showVideoForExercise(wrapper, currentName);
                appState.lastOpenKey = key;
            }
            updateLayout();
        };

        wrapper.appendChild(header);
        wrapper.appendChild(videoCont);
        wrapper.appendChild(setsRest);
        if (ex.alternatives && ex.alternatives.length > 0) wrapper.appendChild(altList);

        if (appState.lastOpenKey === key) {
            altList.classList.add('open');
            showVideoForExercise(wrapper, currentName);
        }

        itemsContainer.appendChild(wrapper);

        // Trigger animation for NEW cards only
        setTimeout(() => {
            wrapper.classList.add('visible');
        }, 30 + index * 70);
    });

    updateLayout();
}

               function renderSplitChoices() {
    const container = document.getElementById('split-choices');
    container.innerHTML = '';
    let options = [];

    if (appState.sessionsPerWeek === 1) options = ['Entire body'];
    else if (appState.sessionsPerWeek === 2) options = ['Entire body', 'Upper/Lower'];
    else if (appState.sessionsPerWeek === 3) options = ['Entire Body 3', 'Push-Pull-Legs'];
    else if (appState.sessionsPerWeek === 4) options = ['Upper/Lower 4', 'Push-Pull-Legs'];
    else options = ['Push-Pull-Legs'];

    options.forEach(option => {
        const div = document.createElement('div');
        div.className = 'choice';
        div.textContent = option;
        container.appendChild(div);
    });
addPressAnimation();   // Re-apply animation to new elements
    
}

                function renderDays() {
            const container = document.getElementById('day-selector');
            container.innerHTML = '';
            for (let i = 1; i <= appState.sessionsPerWeek; i++) {
                const dayEl = document.createElement('div');
                dayEl.className = `day-square ${i === 1 ? 'active' : ''}`;
                dayEl.dataset.day = i;
                dayEl.textContent = `Day ${i}`;
                
                // NEW: Proper event listener instead of inline onclick
                dayEl.addEventListener('click', () => {
                    switchDay(i);
                });
                
                container.appendChild(dayEl);
            }
        }

                function showPage(pageId) {
    const allPages = document.querySelectorAll('.plan-section');
    const currentActive = document.querySelector('.plan-section.active');
    const target = document.getElementById(pageId);
    
    if (!target) return;

    // Fade out current page
    if (currentActive && currentActive !== target) {
        currentActive.classList.remove('active');
        setTimeout(() => {
            currentActive.style.display = 'none';
        }, 280);
    }

    // Show new page with staggered animation
    setTimeout(() => {
        allPages.forEach(p => p.style.display = 'none');
        
        target.style.display = 'block';
        void target.offsetWidth;   // Force reflow
        
        target.classList.add('active');   // Triggers stagger

        if (pageId === 'page3') renderSplitChoices();
        if (pageId === 'page6') {
            renderDays();
            renderStages();
            setStage(1);
            switchDay(1);
            addPressAnimation();
        }

        updateLayout();
    }, 50);
}

// ====================== SMOOTH NAVIGATION DELAY ======================
function navigateWithDelay(targetPage, delay = 260) {
    setTimeout(() => {
        showPage(targetPage);
    }, delay);
}

        // ====================== PRESS ANIMATION (Fixed Duration) ======================
function addPressAnimation() {
    const elements = document.querySelectorAll('.nav-button, .choice, .day-square, .stage-button, .info-button, .sets-rest-value, .exercise-header');

    elements.forEach(el => {
        // Clean previous listeners
        el.removeEventListener('touchstart', handlePress);
        el.removeEventListener('mousedown', handlePress);

        function handlePress(e) {
    const target = e.currentTarget;
    
    // Skip temporary animation for day squares to prevent stuck state
    if (target.classList.contains('day-square')) {
        return; 
    }
    
    target.classList.add('active');
    setTimeout(() => {
        target.classList.remove('active');
    }, 180);
}

        // Use touchstart on mobile, mousedown on desktop
        if ('ontouchstart' in window) {
            el.addEventListener('touchstart', handlePress, { passive: true });
        } else {
            el.addEventListener('mousedown', handlePress);
        }
    });
}

// Helper functions
function onPressStart(e) {
    e.currentTarget.classList.add('active');
}

function onPressEnd(e) {
    e.currentTarget.classList.remove('active');
}

        // ====================== FINAL INITIALIZATION ======================
        document.addEventListener('DOMContentLoaded', () => {
    attachAllListeners();
    addPressAnimation();
    
    // Properly show first page with animation
    const page1 = document.getElementById('page1');
    page1.style.display = 'block';
    void page1.offsetWidth;           // Force reflow
    page1.classList.add('active');
});
    </script>
</body>
</html>
