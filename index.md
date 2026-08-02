---
layout: workshop
venue: "University at Buffalo, SUNY"   # brief name of host site without address (e.g., "Euphoric State University")
address: "University at Buffalo, SUNY, North Campus"     # full street address of workshop (e.g., "Room A, 123 Forth Street, Blimingen, Euphoria")
country: "United States"      # lowercase two-letter ISO country code such as "fr" (see https://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)
language: "en"     # lowercase two-letter ISO language code such as "fr" (see https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
latitude: "43.002890"     # decimal latitude of workshop venue (use https://www.latlong.net/)
longitude: "-78.788780"    # decimal longitude of the workshop venue (use https://www.latlong.net)
humandate: "July 5-11, 2026"    # human-readable dates for the workshop (e.g., "Feb 17-18, 2020")
humantime: "9:00 am - 5:00 pm EDT"    # human-readable times for the workshop (e.g., "9:00 am - 4:30 pm")
startdate: 2026-07-05      # machine-readable start date for the workshop in YYYY-MM-DD format like 2015-01-01
enddate: 2026-07-11        # machine-readable end date for the workshop in YYYY-MM-DD format like 2015-01-02
instructor: ["Alexey Akimov", "Johannes Hachmann", "Ignacio Franco", "Alexander Sokolov", "Benjamin Levine", "Arshad Mehmood" ]  # boxed, comma-separated list of instructors' names as strings, like ["Kay McNulty", "Betty Jennings", "Betty Snyder"]
helper: [ "Layla Heidarizadeh" ] # boxed, comma-separated list of helpers' names, like ["Marlyn Wescoff", "Fran Bilas", "Ruth Lichterman"]
email: ["alexeyak@buffalo.edu"]    # boxed, comma-separated list of contact email addresses for the host, lead instructor, or whoever else is handling questions, like ["marlyn.wescoff@example.org", "fran.bilas@example.org", "ruth.lichterman@example.org"]
collaborative_notes:         # optional: URL for the workshop collaborative notes, e.g. an Etherpad or Google Docs document (e.g., https://pad.carpentries.org/2015-01-01-euphoria)
googleform: https://forms.gle/hTBWeGsqfiQNikwy6
carpentry: "sc"
---


{% comment %}
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
{% endcomment %}


# CyberTraining: Modeling Quantum Dynamics of Excited States in Materials in the Era of Machine Learning, 2026  

## About the Summer School and Workshop  

The 2026 CyberTraining Summer School and Workshop, **Modeling Quantum Dynamics of Excited States in Materials in the Era of Machine Learning,** is 
an intensive, programming-driven training program focused on state-of-the-art theoretical and computational approaches for excited-state and nonadiabatic 
dynamics in molecular and materials systems.

This years theme emphasizes the integration of **quantum dynamics, electronic structure theory, and machine learning (ML)** into unified, 
research-ready workflows. Participants will gain both conceptual foundations and practical, hands-on experience in simulating excited states, charge 
and energy transfer processes, and open quantum system dynamics in atomistic and model systems.

The workshop is designed for graduate students, postdoctoral researchers, faculty, and research scientists working in 
computational chemistry, materials science, chemical physics, and related areas.

---

## Scientific Scope and Themes  

The program will cover foundational and advanced topics including:

- Nonadiabatic dynamics and quantum-classical methods
- Excited-state electronic structure theory
- Quantum dynamics and open quantum systems
- Charge transfer and excitation energy transfer
- Trajectory surface hopping and ab initio multiple spawning (AIMS)
- Hierarchical equations of motion (HEOM)
- Time-dependent density functional theory (TD-DFT) and TD-DFTB
- Correlated electronic structure methods
- Machine learning methods for excited-state modeling
- Algorithm development and numerical methods
- Reproducible workflows, best practices, Git, and GitHub

A strong emphasis will be placed on understanding the **underlying theoretical machinery**, numerical algorithms, and 
implementation details  enabling participants not only to use advanced tools, but also to extend and develop them.

---

## Software and Cyberinfrastructure  

Hands-on sessions will guide participants through practical workflows using modern open-source and 
community-driven software platforms, including:

- ChemML (Hachmann)
- PySCF (Sokolov)
- Prism (Sokolov)
- PySpawn / OpenMolcas (Levine / Mehmood)
- TENSO (Franco, Betancourt, Anderson)
- CP2K / DFTB+ / MOPAC (Akimov)
- Libra (Akimov)

Participants will work directly with Python-based and high-performance computing tools, including PyTorch-enabled 
implementations for quantum dynamics and machine learning integration. The training will cover:

- Computing ground and excited states
- Performing nonadiabatic molecular dynamics simulations
- Coupling electronic structure methods with quantum dynamics
- Data pre-processing, post-processing, and analysis
- Designing ML-enhanced excited-state workflows

---

## Capstone Research Integration

The school will culminate in a **capstone project**, where participants apply the tools and methodologies
learned during the program to a research-relevant problem. These presentations will demonstrate the ability to construct end-to-end
computational workflows for modeling excited-state phenomena in materials and molecular systems

---

## Educational Philosophy  

The CyberTraining program goes beyond a traditional computational chemistry curriculum. Its programming-intensive format equips participants with:

- Deep conceptual understanding of quantum dynamics methods
- Practical experience with advanced research software
- Exposure to method development principles
- Skills for building reproducible, extensible research workflows
- Tools to integrate machine learning into excited-state modeling

By the end of the workshop, participants will be prepared to model, analyze, and extend modern approaches to quantum dynamics of 
excited states in materials  positioning them at the forefront of computational research in the era of machine learning.

The school will leverage the [OnDemand](https://ondemand.ccr.buffalo.edu) gateway at the University at Buffalo

---

## Logistics

{% if page.humandate %}
<p id="when">
  <strong>When:</strong>             
  {{page.humandate}}.
  {% include workshop_calendar.html %}
</p>
{% endif %}

{% if page.latitude and page.longitude %}
<p id="where">
  <strong>Where:</strong>
  {{page.address}}.
  Get directions with
  <a href="//www.openstreetmap.org/?mlat={{page.latitude}}&mlon={{page.longitude}}&zoom=16">OpenStreetMap</a>
  or
  <a href="//maps.google.com/maps?q={{page.latitude}},{{page.longitude}}">Google Maps</a>.
</p>
{% endif %}

{% comment %}
CONTACT EMAIL ADDRESS
Display the contact email address set in the configuration file.
{% endcomment %}
<p id="contact">
  <strong>Contact</strong>:
  Please email
  {% if page.email %}
  {% for email in page.email %}
  {% if forloop.last and page.email.size > 1 %}
  or
  {% else %}
  {% unless forloop.first %}
  ,
  {% endunless %}
  {% endif %}
  <a href='mailto:{{email}}'>{{email}}</a>
  {% endfor %}
  {% else %}
  to-be-announced
  {% endif %}
  for more information.
</p>

### Schedule

{% include base_path.html %}

The details may vary and the order of topics may be changed, the topics may be omitted or added. Please check for the updates. 

  <table class="table table-striped">
  
  <tr>
    <td class="col-md-3"><strong>Date</strong></td>
    <td class="col-md-7"><strong>Topics</strong></td> 
    <td class="col-md-2"><strong>Instructors</strong></td>
  </tr>

  <tr>
    <td class="col-md-3">July 5, 2026, Sunday</td>
    <td class="col-md-7">
      <ul>        
        <li>Arrivals</li>
        <li>Welcome dinner</li>
      </ul>
    </td> 
    <td class="col-md-7">None</td>
  </tr>

  
  <tr>
    <td class="col-md-3">July 6, 2026 (Day 1), Monday, Where: <strong>NSC 201</strong></td>
    <td class="col-md-7">
      <ul>
        <strong>Morning, 9 am - noon</strong>        
        <li><a href="/_episodes/01-introduction">Worshop Kick Off: goals, logistics, details. Overview of the CCR CyberInfrastructure (30 min)</a></li>
        <li><a href="/_episodes/01-introduction">Getting started with the CCR resources. Hands-on (60 min)</a> </li>
        <li><a href="/_episodes/02-ml">General overview of ML computations (Lecture) (30 min)</a></li>
        <li><a href="/_episodes/02-ml">General practice with ML computations (Hands on with PyTorch) (60 min)</a></li>
        <strong>Noon - 1:30 pm</strong> Lunch break
        <strong>Afternoon, 1:30 pm - 5:00 pm</strong>
        <li><a href="/_episodes/02-ml">Theory and demos with ChemML I. Lecture and Demos (50 min)</a></li>
        <li><a href="/_episodes/02-ml">Hands on with ChemML (50 min)</a></li>
        <li><a href="/_episodes/02-ml">Theory and demos with ChemML II. Lecture and Demos (50 min)</a></li>
        <li><a href="/_episodes/02-ml">Hands on with ChemML and working on projects (40 min)</a></li>
        <li><a href="/_episodes/07-project">Working on projects. Collaborations (20 min)</a></li>
      </ul>
    </td> 
    <td class="col-md-2">Alexey Akimov and Johannes Hachmann</td>
  </tr>

  <tr>
    <td class="col-md-3">July 7, 2026 (Day 2), Tuesday, Where: <strong>NSC 210</strong></td>
    <td class="col-md-7">
      <ul>
        <strong>Morning, 9 am - noon</strong>
        <li><a href="/_episodes/03-electronic_structure">Motivation and day overview (25 min)</a></li>
        <li><a href="/_episodes/03-electronic_structure">Testing environment (5 min)</a></li>
        <li><a href="/_episodes/03-electronic_structure">Single-reference methods: theory, practical guidelines, live demos (45 min)</a></li>
        <li><a href="/_episodes/03-electronic_structure">Single-reference methods breakout session (105 min)</a></li>
        <strong>Noon - 1:30 pm</strong> Lunch break
        <strong>Afternoon, 1:30 pm - 5:00 pm</strong>
        <li><a href="/_episodes/03-electronic_structure">Multireference methods: theory, practical guidelines, live demos (45 min)</a></li>
        <li><a href="/_episodes/03-electronic_structure">Multireference methods breakout session (90 min)</a></li>
        <li><a href="/_episodes/03-electronic_structure">Reports from each group (75 min)</a></li>
      </ul>
    </td>
    <td class="col-md-2">Alexander Sokolov</td>
  </tr>

  <tr>
    <td class="col-md-3">July 8, 2026 (Day 3), Wednesday, Where: <strong>Clemens 120</strong></td>
    <td class="col-md-7">
      <ul>
        <strong>Morning, 9 am - noon</strong>        
        <li><a href="/_episodes/03-electronic_structure">Basics of excited state calculations with OpenMolcas. Lecture and Demo (50 min)</a></li>
        <li><a href="/_episodes/03-electronic_structure">Hands on exercises with excited state calculations with OpenMolcas. (60 min)</a></li>
        <li><a href="/_episodes/03-electronic_structure">Advanced electronic structure calculations with OpenMolcas. Lecture and Demo (40 min)</a></li>
        <li><a href="/_episodes/03-electronic_structure">Hands on exercises with excited state calculations with OpenMolcas. (30 min)</a></li>
        <strong>Noon - 1:30 pm</strong> Lunch break
        <strong>Afternoon, 1:30 pm - 5:00 pm</strong>
        <li><a href="/_episodes/04-spawning">Theory of nonadiabatic dynamics with multiple spawning/muliple cloning. Lecture (60 min)</a></li>
        <li><a href="/_episodes/04-spawning">Hands on exercises with PySpawn using model Hamiltonians (60 min)</a></li>
        <li><a href="/_episodes/04-spawning">Demonstration of atomistic AIMS calculations with PySpawn/OpenMolcas (30 min)</a></li>
        <li><a href="/_episodes/04-spawning">Hands on exercises with PySpawn/OpenMolcas (30 min)</a></li>
        <li><a href="/_episodes/07-project">Working on projects. Collaborations (30 min)</a></li>
      </ul>
    </td>
    <td class="col-md-2">Benjamine Levine and Arshad Mehmood</td>
  </tr>

  <tr>
    <td class="col-md-3">July 9, 2026 (Day 4), Thursday, Where: <strong>NSC 201</strong></td>
    <td class="col-md-7">
      <ul>
        <strong>Morning, 9 am - noon</strong>        
        <li><a href="/_episodes/05-tsh">Overview of quantum-classical methodologies. Lecture and demos (100 min)</a></li>
        <li><a href="/_episodes/05-tsh">Hands on exercises with quantum-classical calculations of model Hamiltonians using Libra. (80 min)</a></li>
        <strong>Noon - 1:30 pm</strong> Lunch break
        <strong>Afternoon, 1:30 pm - 5:00 pm</strong>
        <li><a href="/_episodes/05-tsh">Trajectory surface hopping calculation for atomistic systems using Libra interfaces with DFTB+, MOPAC etc. Lecture and demos (60 min)</a></li>
        <li><a href="/_episodes/05-tsh">Hands on exercises on atomistic simulations using Libra (120 min)</a></li>
        <li><a href="/_episodes/07-project">Working on projects. Collaborations (30 min)</a></li>
      </ul>
    </td>
    <td class="col-md-2">Alexey Akimov</td>
  </tr>

  <tr>
    <td class="col-md-3">July 10, 2026 (Day 5), Friday, Where: <strong>Clemens 120</strong></td>
    <td class="col-md-7">
      <ul>
        <strong>Morning, 9 am - noon</strong>        
        <li><a href="/_episodes/06-open_quantum">Theory of quantum dynamics in open systems. Lecture (90 min)</a></li>
        <li><a href="/_episodes/06-open_quantum">Hands on exercis with dissipative quantum dynamics simulations using TENSO (90 min)</a></li>
        <strong>Noon - 1:30 pm</strong> Lunch break
        <strong>Afternoon, 1:30 pm - 5:00 pm</strong>
        <li><a href="/_episodes/06-open_quantum">Hands on exercis with dissipative quantum dynamics simulations using TENSO (30 min)</a></li>
        <li><a href="/_episodes/07-project">Working on projects. Collaborations (120 min)</a></li>
        <strong>Evening 6:30 pm </strong>Optional trip to Niagara Falls
      </ul>
    </td>
    <td class="col-md-2">Ignacio Franco</td>
  </tr>
  
  <tr>
    <td class="col-md-3">July 11, 2023, Saturday</td>
    <td class="col-md-7">Departure</td>
    <td class="col-md-2"></td>
  </tr>
  </table>


---

## Participation
### How to apply to the school

1. Read this page carefully
2. Prepare your application package (you will need it in the next steps)

   2.1. your CV (including graduate or undergraduate GPA)

   2.2. a statement of purpose PDF should describe in no more than 2 pages:

   * your current/ongoing research projects and interests; 
   * how you plan to use the CyberTraining skills gained in this summer school/workshop in your research, for instance do you expect using any of the
     packages that will be covered at this workshop? (see the agenda);
   * propose at least one potential (mini)project to be completed during and shortly after the summer school; the project will be presented 1 week after 
     the event. It should leverage one or more tools/software covered during the workshop (see the agenda). The quality and feasibility 
     of the proposed workshop projects will be considered during the selection of the participants. You can propose more than one project. In any case,
     it should be doable within a short time period (initiate during the school, develop and complete once you return home).
         
   2.3. request your advisor to submit a letter of recommendation for you to the following email: "alexeyak AT buffalo DOT edu", 
   please replace "AT" and "DOT" with the corresponding characters

3. Complete the <a href="https://forms.gle/hTBWeGsqfiQNikwy6" target="_blank" rel="nofollow">**Registration form**</a>


### Important Information about your travel

   * The travel/meal expenses within US per-diem/travel rates will be covered by the workshop for the domestic applicants
   * Travel expenses for a limited number of international applicants can be covered as well
   * For international applicants: you should be able to enter the US. 
   * You will be provided with lodging in a walking distance from the campus
   * UB applicants are also eligible to apply, but your expenses can not be covered by the grant supporting this workshop

### Important dates

   * Workshop application materials are due 5 pm EDT, May 4, 2026
   * Students and Postdocs will be notified of their admission by May 9, 2026
   * Workshop starts: 9 am EDT, July 6, 2026 (first working day)
   * Workshop ends: 5 pm EDT, June 10, 2026 (last working day)


### Who Can Apply

This summer school is primarily intended for graduate students, postdoctoral researchers, and early-career faculty working in 
computational modeling of excited states and nonadiabatic dynamics, in both abstract model systems and atomistic materials applications. 
In exceptional cases, highly motivated undergraduate students with relevant background preparation may also be considered.

The program is particularly suited for researchers engaged in:

- Method development in nonadiabatic, quantum-classical, or quantum dynamics
- Electronic structure theory for excited states
- Applied modeling of photoactive and energy materials (e.g., photovoltaics, photocatalysis, light-harvesting systems)
- Machine learning approaches for quantum and excited-state simulations

Postdoctoral researchers and faculty members seeking hands-on experience with modern simulation tools, software ecosystems, and 
reproducible workflows  as well as deeper conceptual understanding of excited-state and nonadiabatic methods  are strongly encouraged to apply.


### Selection and Restrictions

- **Competitive selection.** Applications will be evaluated based on the strength and clarity of the statement of purpose, 
  the level of fundamental preparation, and the quality of the supervisor's support (when applicable). Prior experience with specialized 
  software or advanced nonadiabatic methods is **not** required. More important is the applicants demonstrated motivation, readiness to learn, 
  ability to engage intensively in a programming-driven environment, and the anticipated impact of the training on their future research, teaching, or career development.

- **Capacity.** The 2026 school will be held in an in-person format. Participation is limited to up to 20 in-person attendees (excluding instructors), 
  ensuring an interactive, hands-on learning environment with close mentorship and collaboration.


---

## Acknowledgement

This workshop is made possible by the NSF-OAC CyberTraining program. Thank you!



{% include links.md %}
