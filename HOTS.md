<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Heroes of the Storm Hero Builder</title>
  <style>
    :root {
      color-scheme: dark;
      --bg: #11151a;
      --panel: #19222b;
      --panel-strong: #1f3245;
      --text: #edf2f8;
      --muted: #9bb3c8;
      --accent: #6dc4ff;
      --accent-strong: #43a7ff;
      --danger: #ff5b5b;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, rgba(56, 147, 255, 0.14), transparent 32%),
        linear-gradient(180deg, #0d1118 0%, #11151a 100%);
      color: var(--text);
    }

    header {
      padding: 1.5rem 2rem;
      border-bottom: 1px solid rgba(255, 255, 255, 0.08);
      display: grid;
      gap: 0.75rem;
      max-width: 1200px;
      margin: 0 auto;
    }

    header h1 {
      margin: 0;
      font-size: clamp(2rem, 2.6vw, 3rem);
    }

    header p {
      margin: 0;
      color: var(--muted);
      max-width: 54rem;
      line-height: 1.65;
    }

    .wrapper {
      max-width: 1200px;
      margin: 2rem auto 3rem;
      padding: 0 2rem;
      display: grid;
      gap: 1.5rem;
    }

    .hero-selector,
    .hero-panel,
    .build-panel,
    .save-panel {
      background: var(--panel);
      border: 1px solid rgba(255, 255, 255, 0.08);
      border-radius: 24px;
      padding: 1.6rem;
      box-shadow: 0 24px 60px rgba(4, 19, 37, 0.18);
    }

    .hero-selector select,
    .hero-panel select,
    .save-panel button,
    .hero-panel button,
    .build-panel button {
      font: inherit;
      color: var(--text);
      border: 1px solid rgba(255, 255, 255, 0.12);
      border-radius: 14px;
      background: rgba(255, 255, 255, 0.04);
      padding: 0.9rem 1rem;
      width: 100%;
      transition: border-color 0.2s ease, background 0.2s ease;
    }

    .hero-selector select:hover,
    .hero-panel select:hover,
    .save-panel button:hover,
    .hero-panel button:hover,
    .build-panel button:hover {
      border-color: rgba(109, 196, 255, 0.45);
      background: rgba(109, 196, 255, 0.08);
    }

    .hero-grid {
      display: grid;
      gap: 1.5rem;
    }

    .columns-2 {
      grid-template-columns: 1fr 0.95fr;
    }

    .hero-summary {
      display: grid;
      gap: 1rem;
    }

    .hero-summary p,
    .info-box p {
      margin: 0;
      line-height: 1.65;
      color: var(--muted);
    }

    .hero-summary h2,
    .hero-summary h3 {
      margin: 0;
      color: var(--text);
    }

    .info-row {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 1rem;
    }

    .info-box {
      padding: 1rem;
      background: rgba(255, 255, 255, 0.03);
      border-radius: 18px;
      border: 1px solid rgba(255, 255, 255, 0.06);
    }

    .talent-tier {
      margin-bottom: 1.25rem;
      padding: 1rem;
      border-radius: 20px;
      background: var(--panel-strong);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }

    .talent-tier h4 {
      margin: 0 0 0.8rem;
      display: flex;
      align-items: center;
      gap: 0.75rem;
      font-size: 1rem;
      letter-spacing: 0.03em;
      text-transform: uppercase;
      color: var(--accent);
    }

    .talent-option {
      display: grid;
      grid-template-columns: minmax(0, 1fr);
      gap: 0.75rem;
    }

    .talent-label {
      display: block;
      padding: 1rem;
      border-radius: 16px;
      border: 1px solid rgba(255, 255, 255, 0.08);
      cursor: pointer;
      transition: background 0.2s ease, border-color 0.2s ease;
    }

    .talent-label:hover {
      border-color: rgba(109, 196, 255, 0.35);
      background: rgba(109, 196, 255, 0.08);
    }

    .talent-label input {
      margin-right: 0.75rem;
    }

    .talent-label strong {
      display: block;
      margin-bottom: 0.4rem;
      color: var(--text);
    }

    .talent-label span {
      display: block;
      color: var(--muted);
      line-height: 1.5;
    }

    .build-panel {
      display: grid;
      gap: 1rem;
    }

    .build-summary,
    .save-panel {
      background: var(--panel-strong);
    }

    .summary-list,
    .saved-builds {
      list-style: none;
      margin: 0;
      padding: 0;
      display: grid;
      gap: 0.75rem;
    }

    .summary-item,
    .save-item {
      padding: 1rem;
      border-radius: 16px;
      background: rgba(255, 255, 255, 0.03);
      border: 1px solid rgba(255, 255, 255, 0.07);
      display: grid;
      gap: 0.45rem;
    }

    .summary-item h4,
    .save-item h4 {
      margin: 0;
      color: var(--accent);
      font-size: 0.95rem;
    }

    .summary-item p,
    .save-item p {
      margin: 0;
      color: var(--muted);
      font-size: 0.95rem;
    }

    .summary-actions {
      display: flex;
      gap: 0.75rem;
      flex-wrap: wrap;
    }

    .save-panel button,
    .save-panel .save-control {
      width: auto;
      min-width: 150px;
    }

    .danger {
      color: var(--danger);
      border-color: rgba(255, 91, 91, 0.22);
    }

    .hero-info-grid {
      display: grid;
      gap: 1rem;
    }

    .hero-info-grid .info-box {
      min-height: 164px;
    }

    @media (min-width: 1000px) {
      .hero-grid {
        grid-template-columns: 1.2fr 0.95fr;
      }
    }

    @media (max-width: 760px) {
      .wrapper {
        padding: 0 1rem;
      }

      .hero-selector button,
      .save-panel button,
      .hero-panel button,
      .hero-panel select {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <header>
    <h1>Heroes of the Storm Hero Builder</h1>
    <p>Design a hero build with skill, trait, and talent choices. Save designs locally, compare net benefits, and review the final build all in one page.</p>
  </header>

  <main class="wrapper">
    <section class="hero-selector">
      <label for="hero-select"><strong>Choose your hero</strong></label>
      <select id="hero-select"></select>
    </section>

    <section class="hero-panel">
      <div class="hero-grid">
        <div class="hero-summary">
          <h2 id="hero-name"></h2>
          <p id="hero-role"></p>
          <p id="hero-description"></p>
        </div>
        <div class="hero-info-grid">
          <div class="info-box">
            <h3>Trait</h3>
            <p id="hero-trait"></p>
          </div>
          <div class="info-box">
            <h3>Core Abilities</h3>
            <div id="hero-abilities"></div>
          </div>
        </div>
      </div>
    </section>

    <section class="hero-panel" id="talent-tree"></section>

    <section class="build-panel">
      <div class="build-summary">
        <h3>Build Summary</h3>
        <ul class="summary-list" id="build-summary"></ul>
      </div>
      <div class="save-panel">
        <div class="summary-actions">
          <button id="save-build">Save build</button>
          <button id="clear-build" class="danger">Reset build</button>
        </div>
        <h3>Saved Builds</h3>
        <ul class="saved-builds" id="saved-builds"></ul>
      </div>
    </section>
  </main>

  <script>
    const heroData = {
      "Jaina": {
        role: "Ranged Assassin",
        description: "A classic burst caster who freezes enemies and controls space with Blizzard.",
        trait: "Frostbitten: Auto attacks reduce the target's movement speed by 25% for 2 seconds.",
        abilities: [
          {name: "Frostbolt", description: "Launches a bolt that deals damage and slows the first enemy hit."},
          {name: "Cone of Cold", description: "Deals damage in a cone and slows all enemies caught."},
          {name: "Blizzard", description: "Drops an area of ice that deals damage over time and slows enemies."},
          {name: "Summon Water Elemental", description: "Summons an elemental that can root enemies on attack."}
        ],
        talents: [
          {
            tier: 1,
            label: "Level 1",
            options: [
              {id: "glacial-evidence", name: "Glacial Evidence", description: "Frostbolt applies Frostbite to enemies for 2 seconds.", effects: {"Frostbite Duration": 2}},
              {id: "burning-spirits", name: "Burning Spirits", description: "Frostbolt deals 15% additional damage to targets already slowed.", effects: {"Damage vs Slowed": "+15%"}},
              {id: "ice-flow", name: "Ice Flow", description: "Gain 5% Spell Power after casting a basic ability, stacking up to 3 times.", effects: {"Spell Power": "+15% max"}}
            ]
          },
          {
            tier: 4,
            label: "Level 4",
            options: [
              {id: "frost-lance", name: "Frost Lance", description: "Frostbolt penetrates and hits additional enemies.", effects: {"Ability Spread": "+1 target"}},
              {id: "cone-of-cold", name: "Cone of Cold", description: "Increases Cone of Cold range and slows slightly more.", effects: {"Range": "+30%", "Slow": "+5%"}},
              {id: "frost-shield", name: "Frost Shield", description: "Casting an ability grants a shield for 2 seconds.", effects: {"Shield": "2 sec"}}
            ]
          },
          {
            tier: 7,
            label: "Level 7",
            options: [
              {id: "ice-block", name: "Ice Block", description: "Reactively block all damage for 1 second once every 90 seconds.", effects: {"Survivability": "+1 block"}},
              {id: "ring-of-frost", name: "Ring of Frost", description: "Adds a short root to Frostbolt's primary target.", effects: {"Crowd Control": "+1 root"}},
              {id: "advanced-snowstorm", name: "Advanced Snowstorm", description: "Blizzard lasts 1 second longer and deals more damage.", effects: {"Duration": "+1 sec", "Damage": "+10%"}}
            ]
          },
          {
            tier: 10,
            label: "Heroic",
            options: [
              {id: "summon-water-elemental", name: "Summon Water Elemental", description: "Summon an elemental that fires icy blasts and can root.", effects: {"Heroic": "Summon Elemental"}},
              {id: "blizzard", name: "Blizzard", description: "Create a damaging area that slows enemies and covers the ground in ice.", effects: {"Heroic": "Area control"}}
            ]
          },
          {
            tier: 13,
            label: "Level 13",
            options: [
              {id: "icebreaker", name: "Icebreaker", description: "Basic Attacks against chilled targets deal 25% bonus damage.", effects: {"Damage": "+25% vs chilled"}},
              {id: "arcane-intellect", name: "Arcane Intellect", description: "Increases maximum mana and mana regeneration.", effects: {"Mana": "+30", "Regen": "+20%"}},
              {id: "frigid-empowerment", name: "Frigid Empowerment", description: "Your next basic ability is empowered after casting Blizzard.", effects: {"Empower": "Next ability"}}
            ]
          },
          {
            tier: 16,
            label: "Level 16",
            options: [
              {id: "polar-chill", name: "Polar Chill", description: "Enemies hit by Cone of Cold are slowed again after 1.5 seconds.", effects: {"Sustained Slow": "+1 proc"}},
              {id: "wintermute", name: "Wintermute", description: "Summon Water Elemental gains damage and attack speed temporarily.", effects: {"Burst": "+30% damage"}},
              {id: "frost-shock", name: "Frost Shock", description: "After using a Heroic ability, your next ability deals 20% bonus damage.", effects: {"Follow-up Damage": "+20%"}}
            ]
          },
          {
            tier: 20,
            label: "Level 20",
            options: [
              {id: "icy-veins", name: "Icy Veins", description: "Gain a burst of Spell Power and cooldown reduction after hitting 3 heroes with Frostbite.", effects: {"Burst": "+20% Power"}},
              {id: "heavy-blow", name: "Heavy Blow", description: "While Water Elemental is active, reduce enemy armor by 10%.", effects: {"Armor reduction": "10%"}},
              {id: "glacial-advance", name: "Glacial Advance", description: "Blizzard becomes an empowered wall that slows enemies further.", effects: {"Area control": "Stronger slow"}}
            ]
          }
        ]
      },
      "Raynor": {
        role: "Ranged Assassin",
        description: "A versatile marksman who excels at sustained damage and objective control.",
        trait: "Inspire: Basic attacks heal Raynor over time and grant Attack Speed.",
        abilities: [
          {name: "Penetrating Round", description: "A long-range shot that deals damage to the first enemy hit."},
          {name: "Running Shot", description: "Fire while moving and gain a burst of movement speed."},
          {name: "Inspire", description: "Basic attacks heal Raynor and increase attack speed over time."},
          {name: "Hyperion", description: "Call down a flying siege unit that bombards enemies."}
        ],
        talents: [
          {
            tier: 1,
            label: "Level 1",
            options: [
              {id: "double-barrel", name: "Double Barrel", description: "Penetrating Round deals 15% more damage and hits 2 additional targets.", effects: {"Damage": "+15%", "Targets": "+2"}},
              {id: "replenishment", name: "Replenishment", description: "Basic Attacks restore 4 mana.", effects: {"Mana sustain": "+4 per attack"}},
              {id: "power-surge", name: "Power Surge", description: "Using Running Shot increases your damage by 20% for 3 seconds.", effects: {"Damage buff": "+20%"}}
            ]
          },
          {
            tier: 4,
            label: "Level 4",
            options: [
              {id: "condensed-energy", name: "Condensed Energy", description: "Inspire heals for more and unlocks a stronger heal at 10 stacks.", effects: {"Healing": "+25%"}},
              {id: "unstoppable-hero", name: "Unstoppable Hero", description: "Running Shot grants Unstoppable for 1 second.", effects: {"Mobility": "+Unstoppable"}},
              {id: "advancing-strike", name: "Advancing Strike", description: "Penetrating Round reduces enemy Armor by 10% for 3 seconds.", effects: {"Armor reduction": "10%"}}
            ]
          },
          {
            tier: 7,
            label: "Level 7",
            options: [
              {id: "penetrating-shots", name: "Penetrating Shots", description: "Penetrating Round pierces and has increased range.", effects: {"Range": "+35%", "Pierce": "Yes"}},
              {id: "second-wind", name: "Second Wind", description: "Gain bonus Attack Speed after your health drops below 50%.", effects: {"Attack Speed": "+15%"}},
              {id: "battering-ram", name: "Battering Ram", description: "Running Shot reduces enemy movement speed after hitting them.", effects: {"Slow": "15%"}}
            ]
          },
          {
            tier: 10,
            label: "Heroic",
            options: [
              {id: "hyperion", name: "Hyperion", description: "Summon a flying siege unit that deals massive damage to enemies in its path.", effects: {"Heroic": "Siege artillery"}},
              {id: "stimpacks", name: "Stim Packs", description: "Gain a burst of Attack Speed and Movement Speed for 8 seconds.", effects: {"Heroic": "Burst speed"}}
            ]
          },
          {
            tier: 13,
            label: "Level 13",
            options: [
              {id: "anti-armor-shells", name: "Anti-Armor Shells", description: "Penetrating Round reduces Armor by 20% for 4 seconds.", effects: {"Armor reduction": "20%"}},
              {id: "relics-of-raynor", name: "Relics of Raynor", description: "Inspire increases Attack Speed by an additional 10%.", effects: {"Attack Speed": "+10%"}},
              {id: "sharpshooter", name: "Sharpshooter", description: "Gain 10% critical strike chance after using Running Shot.", effects: {"Critical Chance": "+10%"}}
            ]
          },
          {
            tier: 16,
            label: "Level 16",
            options: [
              {id: "collective-cannon", name: "Collective Cannon", description: "Penetrating Round fires three shots in quick succession.", effects: {"Burst": "Triple shot"}},
              {id: "banshee-consume", name: "Banshee Consume", description: "Hyperion slows enemies and deals extra damage over time.", effects: {"Slow": "20%", "Damage": "+30%"}},
              {id: "combat-focus", name: "Combat Focus", description: "Every 4th Basic Attack deals 50% bonus damage.", effects: {"Bonus damage": "+50%"}}
            ]
          },
          {
            tier: 20,
            label: "Level 20",
            options: [
              {id: "terminator", name: "Terminator", description: "Gain 25% Attack Speed and Spell Armor while Hyperion is active.", effects: {"Attack Speed": "+25%", "Armor": "+Spell Armor"}},
              {id: "brutal-strike", name: "Brutal Strike", description: "Penetrating Round deals 50% more damage to Heroes.", effects: {"Hero damage": "+50%"}},
              {id: "rapid-fire", name: "Rapid Fire", description: "Basic Attacks fire 30% faster for 4 seconds after using Inspire twice.", effects: {"Attack Speed": "+30%"}}
            ]
          }
        ]
      },
      "Tyrande": {
        role: "Support",
        description: "A ranged support focused on healing, burst damage and long-range crowd control.",
        trait: "Priestess of the Moon: Basic Attacks deal bonus damage and reveal the target.",
        abilities: [
          {name: "Light of Elune", description: "Heal an ally or damage an enemy, depending on target."},
          {name: "Shadowstalk", description: "Restore health to nearby allies and become stealthed."},
          {name: "Sentinel", description: "Send a flying owl that reveals and slows enemies."},
          {name: "Lunar Flare", description: "Call down a beam of moonlight that damages and stuns after a delay."}
        ],
        talents: [
          {
            tier: 1,
            label: "Level 1",
            options: [
              {id: "illuminate", name: "Illuminate", description: "Light of Elune's damage and healing increase by 10%.", effects: {"Healing": "+10%", "Damage": "+10%"}},
              {id: "mark-of-the-warden", name: "Mark of the Warden", description: "Priestess of the Moon applies a slow for 25%.", effects: {"Slow": "25%"}},
              {id: "tyrande-siphon", name: "Tyrande's Siphon", description: "Gain mana back for hitting Heroes with Shadowstalk.", effects: {"Mana sustain": "Improved"}}
            ]
          },
          {
            tier: 4,
            label: "Level 4",
            options: [
              {id: "elune's-blessing", name: "Elune's Blessing", description: "Light of Elune heals for an extra 50% of the damage dealt.", effects: {"Healing": "+50% of damage"}},
              {id: "vow-of-the-warden", name: "Vow of the Warden", description: "Sentinel's slow lasts 2 seconds longer.", effects: {"Slow Duration": "+2 sec"}},
              {id: "moonlit-arrows", name: "Moonlit Arrows", description: "Basic Attacks reduce Light of Elune cooldown by 0.5 seconds.", effects: {"Cooldown reduction": "0.5 sec"}}
            ]
          },
          {
            tier: 7,
            label: "Level 7",
            options: [
              {id: "guided-archery", name: "Guided Archery", description: "Increase attack range and damage of basic attacks.", effects: {"Range": "+20%", "Damage": "+15%"}},
              {id: "sentinel-wings", name: "Sentinel Wings", description: "Sentinel gains bonus movement speed and a stun on hit.", effects: {"Mobility": "+speed", "CC": "+stun"}},
              {id: "ranger's-closing", name: "Ranger's Closing", description: "After Shadowstalk ends, Light of Elune's cooldown is reduced by 2 seconds.", effects: {"Cooldown reduction": "2 sec"}}
            ]
          },
          {
            tier: 10,
            label: "Heroic",
            options: [
              {id: "shadowstalk", name: "Shadowstalk", description: "Restore health to allies and grant stealth to your team.", effects: {"Heroic": "Team stealth"}},
              {id: "lunar-flare", name: "Lunar Flare", description: "Damage and stun enemies in a large area after a brief delay.", effects: {"Heroic": "AoE stun"}}
            ]
          },
          {
            tier: 13,
            label: "Level 13",
            options: [
              {id: "starfallen", name: "Starfallen", description: "Light of Elune heals for 30% more if the target is below 35% health.", effects: {"Low health healing": "+30%"}},
              {id: "feathered-justice", name: "Feathered Justice", description: "Lunar Flare's stun duration is increased by 0.5 seconds.", effects: {"Stun Duration": "+0.5 sec"}},
              {id: "roar-of-the-owls", name: "Roar of the Owls", description: "Basic Attacks with Sentinel deal bonus damage to Heroes.", effects: {"Hero damage": "+20%"}}
            ]
          },
          {
            tier: 16,
            label: "Level 16",
            options: [
              {id: "owl's-bite", name: "Owl's Bite", description: "Sentinel deals splash damage around the target it hits.", effects: {"Splash": "Yes"}},
              {id: "mark-of-illumination", name: "Mark of Illumination", description: "Enemies hit by Light of Elune take 5% increased damage from your team.", effects: {"Team damage": "+5%"}},
              {id: "moonlit-prowess", name: "Moonlit Prowess", description: "Gain Spell Power after landing Lunar Flare on a Hero.", effects: {"Spell Power": "+15%"}}
            ]
          },
          {
            tier: 20,
            label: "Level 20",
            options: [
              {id: "starlight-refraction", name: "Starlight Refraction", description: "Shadowstalk also grants nearby allies damage reduction.", effects: {"Damage reduction": "15%"}},
              {id: "lunar-eclipse", name: "Lunar Eclipse", description: "Lunar Flare blasts a larger area and stuns for 1 second longer.", effects: {"Area": "+25%", "Stun": "+1 sec"}},
              {id: "guardian-spirit", name: "Guardian Spirit", description: "After using Shadowstalk, the next Light of Elune is instant.", effects: {"Cast speed": "Instant"}}
            ]
          }
        ]
      }
    };

    const heroSelect = document.getElementById("hero-select");
    const heroName = document.getElementById("hero-name");
    const heroRole = document.getElementById("hero-role");
    const heroDescription = document.getElementById("hero-description");
    const heroTrait = document.getElementById("hero-trait");
    const heroAbilities = document.getElementById("hero-abilities");
    const talentTree = document.getElementById("talent-tree");
    const buildSummary = document.getElementById("build-summary");
    const savedBuildsList = document.getElementById("saved-builds");
    const saveBuildButton = document.getElementById("save-build");
    const clearBuildButton = document.getElementById("clear-build");

    let savedBuilds = {};
    const storageKey = "hotsHeroBuilderSaves";

    function loadSavedBuilds() {
      const raw = localStorage.getItem(storageKey);
      savedBuilds = raw ? JSON.parse(raw) : {};
    }

    function saveSavedBuilds() {
      localStorage.setItem(storageKey, JSON.stringify(savedBuilds));
    }

    function populateHeroSelect() {
      heroSelect.innerHTML = Object.keys(heroData)
        .map(hero => `<option value="${hero}">${hero}</option>`)
        .join("");
    }

    function getCurrentBuild() {
      const hero = heroSelect.value;
      const heroInfo = heroData[hero];
      const selection = { hero, talents: {} };
      heroInfo.talents.forEach(tier => {
        const selected = document.querySelector(`input[name="tier-${tier.tier}"]:checked`);
        selection.talents[tier.tier] = selected ? selected.value : null;
      });
      return selection;
    }

    function renderHeroInfo() {
      const hero = heroData[heroSelect.value];
      heroName.textContent = heroSelect.value;
      heroRole.textContent = hero.role;
      heroDescription.textContent = hero.description;
      heroTrait.textContent = hero.trait;
      heroAbilities.innerHTML = hero.abilities
        .map(item => `<p><strong>${item.name}</strong>: ${item.description}</p>`)
        .join("");
    }

    function renderTalentTree() {
      const hero = heroData[heroSelect.value];
      talentTree.innerHTML = hero.talents
        .map(tier => {
          const options = tier.options
            .map(option => `
              <label class="talent-label" for="${tier.tier}-${option.id}">
                <input type="radio" id="${tier.tier}-${option.id}" name="tier-${tier.tier}" value="${option.id}" />
                <strong>${option.name}</strong>
                <span>${option.description}</span>
              </label>
            `)
            .join("");
          return `
            <section class="talent-tier">
              <h4>${tier.label}</h4>
              <div class="talent-option">${options}</div>
            </section>
          `;
        })
        .join("");

      talentTree.querySelectorAll("input[type=radio]").forEach(input => {
        input.addEventListener("change", renderBuildSummary);
      });

      // Default to first option for each tier
      hero.talents.forEach(tier => {
        const firstOption = document.querySelector(`input[name="tier-${tier.tier}"]`);
        if (firstOption && !document.querySelector(`input[name="tier-${tier.tier}"]:checked`)) {
          firstOption.checked = true;
        }
      });
    }

    function aggregateEffects(selection) {
      const hero = heroData[selection.hero];
      const effects = {};

      hero.talents.forEach(tier => {
        const talentId = selection.talents[tier.tier];
        const chosen = tier.options.find(option => option.id === talentId);
        if (!chosen) return;
        Object.entries(chosen.effects).forEach(([key, value]) => {
          if (!effects[key]) {
            effects[key] = [];
          }
          effects[key].push(value);
        });
      });
      return effects;
    }

    function renderBuildSummary() {
      const selection = getCurrentBuild();
      const hero = heroData[selection.hero];
      const summaryItems = [];

      hero.talents.forEach(tier => {
        const talentId = selection.talents[tier.tier];
        const chosen = tier.options.find(option => option.id === talentId);
        if (!chosen) return;
        summaryItems.push(`
          <li class="summary-item">
            <h4>${tier.label}: ${chosen.name}</h4>
            <p>${chosen.description}</p>
          </li>
        `);
      });

      const aggregated = aggregateEffects(selection);
      const effectLines = Object.entries(aggregated)
        .map(([key, values]) => `<li class="summary-item"><h4>${key}</h4><p>${values.join(" + ")}</p></li>`)
        .join("");

      buildSummary.innerHTML = `
        <li class="summary-item"><h4>Hero</h4><p>${selection.hero} — ${hero.role}</p></li>
        ${summaryItems.join("")}
        ${effectLines ? `<li class="summary-item"><h4>Net Benefits</h4><p>${Object.entries(aggregated)
          .map(([key, values]) => `${key}: ${values.join(" + ")}`)
          .join(" • ")}</p></li>` : ""}
      `;
    }

    function renderSavedBuilds() {
      const entries = Object.entries(savedBuilds);
      if (!entries.length) {
        savedBuildsList.innerHTML = `<li class="save-item"><p>No saved builds yet. Save your first design above.</p></li>`;
        return;
      }
      savedBuildsList.innerHTML = entries
        .map(([name, build]) => `
          <li class="save-item">
            <h4>${name}</h4>
            <p>${build.hero} — ${Object.keys(build.talents).length} choices selected</p>
            <div class="summary-actions">
              <button onclick="loadSavedBuild('${encodeURIComponent(name)}')">Load</button>
              <button class="danger" onclick="deleteSavedBuild('${encodeURIComponent(name)}')">Delete</button>
            </div>
          </li>
        `)
        .join("");
    }

    function updateBuildUI() {
      renderHeroInfo();
      renderTalentTree();
      renderBuildSummary();
      renderSavedBuilds();
    }

    function loadSavedBuild(encodedName) {
      const name = decodeURIComponent(encodedName);
      const build = savedBuilds[name];
      if (!build) return;
      heroSelect.value = build.hero;
      updateBuildUI();

      Object.entries(build.talents).forEach(([tier, talentId]) => {
        const input = document.querySelector(`input[name="tier-${tier}"][value="${talentId}"]`);
        if (input) {
          input.checked = true;
        }
      });
      renderBuildSummary();
    }

    window.loadSavedBuild = loadSavedBuild;

    function deleteSavedBuild(encodedName) {
      const name = decodeURIComponent(encodedName);
      delete savedBuilds[name];
      saveSavedBuilds();
      renderSavedBuilds();
    }

    saveBuildButton.addEventListener("click", () => {
      const selection = getCurrentBuild();
      const name = prompt("Name this build:", `${selection.hero} build`);
      if (!name) return;
      savedBuilds[name] = selection;
      saveSavedBuilds();
      renderSavedBuilds();
      alert(`Saved build: ${name}`);
    });

    clearBuildButton.addEventListener("click", () => {
      if (!confirm("Reset the current build to defaults?")) return;
      renderTalentTree();
      renderBuildSummary();
    });

    heroSelect.addEventListener("change", updateBuildUI);

    loadSavedBuilds();
    populateHeroSelect();
    updateBuildUI();
  </script>
</body>
</html>
