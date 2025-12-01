import streamlit as st
import heapq

# -----------------------------------------------------------
#                🌙 NEW MODERN UI — BLUE THEME
# -----------------------------------------------------------

st.set_page_config(
    page_title="Kidney Disease Diagnostic Assistant",
    page_icon="💧",
    layout="wide"
)

st.markdown(
    """
    <style>
        body {
            background-color: #f0f4fa;
        }
        .main-title {
            text-align:center;
            font-size: 46px;
            font-weight: 800;
            color: #0a3d62;
            padding-top:10px;
        }
        .subtitle {
            text-align:center;
            font-size: 20px;
            color:#4b6584;
            margin-bottom:30px;
        }

        .hero {
            background: linear-gradient(135deg, #1B98E0 0%, #0a3d62 100%);
            padding: 40px;
            border-radius: 20px;
            color:white;
            margin-bottom: 45px;
            text-align: center;
        }

        .hero h1 {
            font-size: 50px;
            font-weight: 800;
        }

        .input-card {
            background:white;
            padding:25px;
            border-radius: 16px;
            box-shadow:0 4px 12px rgba(0,0,0,0.1);
        }

        .result-card {
            background:white;
            padding:20px;
            border-radius:16px;
            border-left: 6px solid #1B98E0;
            box-shadow:0 3px 10px rgba(0,0,0,0.08);
            margin-bottom:20px;
        }

        .disease-title {
            font-size: 26px;
            font-weight:700;
            color:#0a3d62;
            margin-bottom:6px;
        }

        .score {
            font-size: 18px;
            font-weight:600;
            color:#1B98E0;
        }
    </style>
    """,
    unsafe_allow_html=True
)

# Sidebar Navigation
st.sidebar.title("🔍 Navigation")
st.sidebar.markdown("Use the options below:")
st.sidebar.button("Home")
st.sidebar.button("Kidney Disease Info")
st.sidebar.button("About System")


# -----------------------------------------------------------
#                 💧 HERO BANNER
# -----------------------------------------------------------

st.markdown(
    """
    <div class="hero">
        <h1>Kidney Diagnostic Assistant</h1>
        <p>AI-powered A* reasoning for faster kidney health assessment</p>
    </div>
    """,
    unsafe_allow_html=True
)


# -----------------------------------------------------------
#           🧠 NEW KIDNEY DISEASE KNOWLEDGE GRAPH
# -----------------------------------------------------------

medical_graph = {
    "Acute Kidney Injury (AKI)": {
        "symptoms": ["fatigue", "decreased urine", "swelling", "confusion", "nausea"],
        "severity": 0.9,
        "next_steps": "Immediate medical attention required. Check creatinine levels, urine tests, IV fluids."
    },
    "Chronic Kidney Disease (CKD)": {
        "symptoms": ["fatigue", "swelling", "ankle swelling", "poor appetite", "itching"],
        "severity": 0.8,
        "next_steps": "Long-term monitoring required. Control blood pressure, diabetes, and reduce salt intake."
    },
    "Kidney Stones": {
        "symptoms": ["severe pain", "side pain", "blood urine", "nausea", "vomiting"],
        "severity": 0.7,
        "next_steps": "Drink plenty of water, pain medication, ultrasound/CT scan recommended."
    },
    "Urinary Tract Infection (UTI)": {
        "symptoms": ["burning urine", "frequent urination", "cloudy urine", "lower abdominal pain"],
        "severity": 0.6,
        "next_steps": "Urine culture test, antibiotics, drink more fluids."
    },
    "Glomerulonephritis": {
        "symptoms": ["blood urine", "puffiness", "high BP", "foamy urine", "swelling"],
        "severity": 0.75,
        "next_steps": "Blood/urine tests, kidney biopsy in severe cases. Treat underlying cause."
    }
}


# -----------------------------------------------------------
#              ⭐ A* DIAGNOSTIC ALGORITHM
# -----------------------------------------------------------

def heuristic(symptom_matches: int, total_symptoms: int, severity: float) -> float:
    if total_symptoms == 0:
        return 0
    match_score = (symptom_matches / total_symptoms) * 0.7
    severity_score = severity * 0.3
    return match_score + severity_score


def a_star_diagnosis(user_symptoms):
    pq = []
    results = []

    for disease, data in medical_graph.items():
        matches = len(set(user_symptoms) & set(data["symptoms"]))
        h = heuristic(matches, len(data["symptoms"]), data["severity"])
        heapq.heappush(pq, (-h, disease))

    while pq:
        score, disease = heapq.heappop(pq)
        score = -score
        results.append((disease, score))

    return results


# -----------------------------------------------------------
#                     🧑‍⚕ USER INPUT CARD
# -----------------------------------------------------------

st.markdown("### 📝 Enter Your Symptoms")

with st.container():
    st.markdown("<div class='input-card'>", unsafe_allow_html=True)

    user_input = st.text_area(
        "Describe symptoms (comma separated):",
        placeholder="Example: swelling, fatigue, decreased urine"
    )

    submit = st.button("Run Kidney Diagnosis 💧", type="primary")

    st.markdown("</div>", unsafe_allow_html=True)


# -----------------------------------------------------------
#                      📊 RESULTS
# -----------------------------------------------------------

if submit and user_input.strip() != "":
    symptoms = [s.strip().lower() for s in user_input.split(",")]
    results = a_star_diagnosis(symptoms)

    st.markdown("## 🧪 Diagnosis Results")
    st.markdo
