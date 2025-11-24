# ct-pokemon-giveaway
ct-pokemon-giveaway

script:

<script>
    const LEVEL = 100;

    const pokemonData = {
      "magearna": {
        name: "Magearna",
        types: ["Steel", "Fairy"],
        baseStats: { hp: 80, atk: 95, def: 115, spa: 130, spd: 115, spe: 65 },
        moves: [
          { id: "hyperbeam",    name: "Hyper Beam",    type: "Normal",  category: "Special", power: 150 },
          { id: "fleurcannon",  name: "Fleur Cannon",  type: "Fairy",   category: "Special", power: 130 },
          { id: "focusblast",   name: "Focus Blast",   type: "Fighting",category: "Special", power: 120 },
          { id: "energyball",   name: "Energy Ball",   type: "Grass",   category: "Special", power: 90 },
          { id: "icebeam",      name: "Ice Beam",      type: "Ice",     category: "Special", power: 90 },
          { id: "thunderbolt",  name: "Thunderbolt",   type: "Electric",category: "Special", power: 90 },
          { id: "flashcannon",  name: "Flash Cannon",  type: "Steel",   category: "Special", power: 80 },
          { id: "corkscrewcrash", name: "Corkscrew Crash", type: "Steel", category: "Special", power: 160, zMoveOf: "flashcannon" }
        ],
        opponentMoves: [
          { id: "fleurcannon",  name: "Fleur Cannon",  type: "Fairy",   category: "Special", power: 130 },
          { id: "focusblast",   name: "Focus Blast",   type: "Fighting",category: "Special", power: 120 },
          { id: "icebeam",      name: "Ice Beam",      type: "Ice",     category: "Special", power: 90 },
          { id: "thunderbolt",  name: "Thunderbolt",   type: "Electric",category: "Special", power: 90 }
        ]
      },
      "landorus-t": {
        name: "Landorus-Therian",
        types: ["Ground", "Flying"],
        baseStats: { hp: 89, atk: 145, def: 90, spa: 105, spd: 80, spe: 91 },
        moves: [
          { id: "explosion",    name: "Explosion",     type: "Normal",  category: "Physical", power: 250 },
          { id: "gigaimpact",   name: "Giga Impact",   type: "Normal",  category: "Physical", power: 150 },
          { id: "outrage",      name: "Outrage",       type: "Dragon",  category: "Physical", power: 120 },
          { id: "superpower",   name: "Superpower",    type: "Fighting",category: "Physical", power: 120 },
          { id: "earthquake",   name: "Earthquake",    type: "Ground",  category: "Physical", power: 100 },
          { id: "fly",          name: "Fly",           type: "Flying",  category: "Physical", power: 90 },
          { id: "supersonicskystrike", name: "Supersonic Skystrike", type: "Flying", category: "Physical", power: 175, zMoveOf: "fly" }
        ],
        opponentMoves: [
          { id: "earthquake",   name: "Earthquake",    type: "Ground",  category: "Physical", power: 100 },
          { id: "stoneedge",    name: "Stone Edge",    type: "Rock",    category: "Physical", power: 100 },
          { id: "explosion",    name: "Explosion",     type: "Normal",  category: "Physical", power: 250 },
          { id: "u-turn",       name: "U-turn",        type: "Bug",     category: "Physical", power: 70 }
        ]
      }
    };

    const natures = {
      "Docile":  { plus: null, minus: null },
      "Jolly":   { plus: "spe", minus: "spa" },
      "Adamant": { plus: "atk", minus: "spa" },
      "Careful": { plus: "spd", minus: "spa" },
      "Modest":  { plus: "spa", minus: "atk" },
      "Timid":   { plus: "spe", minus: "atk" },
      "Bold":    { plus: "def", minus: "atk" }
    };

    function natureOptionsFor(pokemonId) {
      return pokemonId === "magearna"
        ? ["Docile", "Modest", "Timid", "Bold"]
        : ["Docile", "Jolly", "Adamant", "Careful"];
    }

    const itemsByPokemon = {
      "magearna": [
        { id: "none",         name: "None",          description: "No Held Item.", effect: {} },
        { id: "choicescarf",  name: "Choice Scarf",  description: "Boosts Speed by 50%.", effect: { speedMultiplier: 1.5 } },
        { id: "expertbelt",   name: "Expert Belt",  description: "Boosts super-effective moves (about 20%).", effect: { expertBelt: true } },
        { id: "pixieplate",   name: "Pixie Plate",   description: "Boosts Fairy-type moves (about 20%).", effect: { typeBoost: { type: "Fairy", multiplier: 1.2 } } },
        { id: "steeliumz",    name: "Steelium Z",    description: "Enables Corkscrew Crash (Z-Flash Cannon).", effect: { steeliumZ: true } },
        { id: "shucaberry",   name: "Shuca Berry",   description: "Halves damage from one super-effective Ground-type move.", effect: { berryType: "Ground" } }
      ],
      "landorus-t": [
        { id: "none",         name: "None",         description: "No Held Item.", effect: {} },
        { id: "choiceband",   name: "Choice Band",  description: "Boosts Attack by 50%.", effect: { attackMultiplier: 1.5 } },
        { id: "expertbelt",   name: "Expert Belt",  description: "Boosts super-effective moves (about 20%).", effect: { expertBelt: true } },
        { id: "focussash",    name: "Focus Sash",   description: "If at full HP, survives a would-be KO with 1 HP.", effect: { focusSash: true } },
        { id: "flyiniumz",    name: "Flyinium Z",   description: "Enables Supersonic Skystrike (Z-Fly) move.", effect: { flyiniumZ: true } }
      ]
    };

    const opponentPresets = {
      "magearna": {
        nature: "Timid",
        evs: { hp: 0, atk: 0, def: 252, spa: 164, spd: 0, spe: 92 },
        itemId: "choicescarf",
        willUseMoveId: "icebeam"
      },
      "landorus-t": {
        nature: "Adamant",
        evs: { hp: 144, atk: 252, def: 0, spa: 0, spd: 0, spe: 0 },
        itemId: "yacheberry",
        willUseMoveId: "earthquake"
      }
    };

    const spritePaths = {
      "magearna": "img/magearna.png",
      "landorus-t": "img/landorus-t.png"
    };

    function getNatureModifier(natureName, stat) {
      if (!natureName || !natures[natureName]) return 1.0;
      const n = natures[natureName];
      if (n.plus === stat) return 1.1;
      if (n.minus === stat) return 0.9;
      return 1.0;
    }
    function calcHPStat(base, ev, level = LEVEL, iv = 31) {
      return Math.floor(((2 * base + iv + Math.floor(ev / 4)) * level) / 100) + level + 10;
    }
    function calcOtherStat(base, ev, natureName, statKey, level = LEVEL, iv = 31) {
      const raw = Math.floor(((2 * base + iv + Math.floor(ev / 4)) * level) / 100) + 5;
      return Math.floor(raw * getNatureModifier(natureName, statKey));
    }

    const typeChart = {
      "Normal": { "Steel": 0.5 },
      "Fairy":  { "Dragon": 2, "Steel": 0.5 },
      "Fighting": { "Steel": 2, "Fairy": 0.5, "Flying": 0.5 },
      "Grass": { "Ground": 2, "Flying": 0.5, "Steel": 0.5 },
      "Dragon": { "Fairy": 0, "Steel": 0.5 },
      "Ice": { "Ground": 2, "Flying": 2, "Steel": 0.5 },
      "Electric": { "Ground": 0, "Flying": 2 },
      "Steel": { "Fairy": 2 },
      "Ground": { "Steel": 2, "Electric": 2, "Flying": 0 },
      "Rock": { "Flying": 2 },
      "Bug": { "Grass": 2 }
    };
    function getTypeEffectiveness(moveType, targetTypes) {
      let mult = 1;
      for (const t of targetTypes) {
        const row = typeChart[moveType];
        if (row && Object.prototype.hasOwnProperty.call(row, t)) mult *= row[t];
      }
      return mult;
    }

    function getItemOffensiveMultipliers(itemId, move, pokemonId) {
      const pool = [
        ...itemsByPokemon["magearna"],
        ...itemsByPokemon["landorus-t"],
        { id:"yacheberry", effect:{ berryType:"Ice" } },
        { id:"choicescarf", effect:{ speedMultiplier:1.5 } }
      ];
      const item = pool.find(i => i.id === itemId);
      if (!item) return { atk: 1, spa: 1, spe: 1, dmg: 1, typeBoost: null, expertBelt: false };
      const eff = item.effect || {};
      return {
        atk: move.category === "Physical" && eff.attackMultiplier ? eff.attackMultiplier : 1,
        spa: move.category === "Special" && eff.spAttackMultiplier ? eff.spAttackMultiplier : 1,
        spe: eff.speedMultiplier || 1,
        dmg: eff.damageMultiplier || 1,
        typeBoost: eff.typeBoost || null,
        expertBelt: !!eff.expertBelt
      };
    }
    function getItemDefensiveEffects(itemId) {
      const pool = [
        ...itemsByPokemon["magearna"],
        ...itemsByPokemon["landorus-t"],
        { id:"yacheberry", effect:{ berryType:"Ice" } }
      ];
      const item = pool.find(i => i.id === itemId);
      if (!item) return {};
      const eff = item.effect || {};
      return { berryType: eff.berryType || null, focusSash: !!eff.focusSash, steeliumZ: !!eff.steeliumZ };
    }

    function calcDamage({ level, attacker, defender, move, attackerItemId }) {
      const category = move.category;
      const atkStat = category === "Physical" ? attacker.atk : attacker.spa;
      const defStat = category === "Physical" ? defender.def : defender.spd;

      const itemMults = getItemOffensiveMultipliers(attackerItemId, move, attacker.id);
      const effectiveAtk = category === "Physical"
        ? atkStat * itemMults.atk
        : atkStat * itemMults.spa;

      let baseDamage = Math.floor(
        Math.floor(
          (((2 * level) / 5 + 2) * move.power * (effectiveAtk / defStat)) / 50
        ) + 2
      );

      const stab = attacker.types.includes(move.type) ? 1.5 : 1.0;
      const typeMult = getTypeEffectiveness(move.type, defender.types) || 1;
      const randomFactor = 0.925;

      let typeBoostMult = 1;
      if (itemMults.typeBoost && itemMults.typeBoost.type === move.type) {
        typeBoostMult *= itemMults.typeBoost.multiplier;
      }

      let expertBeltMult = 1;
      if (itemMults.expertBelt && typeMult > 1) expertBeltMult = 1.2;

      const finalDamage = Math.floor(
        baseDamage * stab * typeMult * randomFactor * itemMults.dmg * typeBoostMult * expertBeltMult
      );

      return { damage: finalDamage, stab, typeMult };
    }

    function buildStatsFor(pokemonId, natureName, evs, itemId) {
      const pData = pokemonData[pokemonId];
      const base = pData.baseStats;

      const hp  = calcHPStat(base.hp,  evs.hp);
      let atk   = calcOtherStat(base.atk, evs.atk, natureName, "atk");
      let def   = calcOtherStat(base.def, evs.def, natureName, "def");
      let spa   = calcOtherStat(base.spa, evs.spa, natureName, "spa");
      let spd   = calcOtherStat(base.spd, evs.spd, natureName, "spd");
      let spe   = calcOtherStat(base.spe, evs.spe, natureName, "spe");

      const multPhys = getItemOffensiveMultipliers(itemId, { category: "Physical" }, pokemonId);
      const multSpec = getItemOffensiveMultipliers(itemId, { category: "Special" }, pokemonId);

      atk = Math.floor(atk * multPhys.atk);
      spa = Math.floor(spa * multSpec.spa);
      spe = Math.floor(spe * Math.max(multPhys.spe, multSpec.spe));

      return { id: pokemonId, name: pData.name, types: pData.types, maxHP: hp, hp, atk, def, spa, spd, spe, base: pData };
    }

    const playerPokemonEl = document.getElementById("playerPokemon");
    const playerNatureEl  = document.getElementById("playerNature");
    const playerItemEl    = document.getElementById("playerItem");
    const playerMoveEl    = document.getElementById("playerMove");
    const playerNameEl    = document.getElementById("playerName");

    const evInputs = {
      hp: document.getElementById("evHP"),
      atk: document.getElementById("evAtk"),
      def: document.getElementById("evDef"),
      spa: document.getElementById("evSpA"),
      spd: document.getElementById("evSpD"),
      spe: document.getElementById("evSpe")
    };
    const evVals = {
      hp: document.getElementById("evHPVal"),
      atk: document.getElementById("evAtkVal"),
      def: document.getElementById("evDefVal"),
      spa: document.getElementById("evSpAVal"),
      spd: document.getElementById("evSpDVal"),
      spe: document.getElementById("evSpeVal")
    };
    const evTotalNoteEl = document.getElementById("evTotalNote");

    const itemInfoEl      = document.getElementById("itemInfo");
    const playerSummaryEl = document.getElementById("playerSummary");
    const opponentSummaryEl = document.getElementById("opponentSummary");
    const playerStatsBody = document.getElementById("playerStatsBody");
    const opponentStatsBody = document.getElementById("opponentStatsBody");
    const playerMovesTableBody = document.getElementById("playerMovesTableBody");
    const speedComparisonNoteEl = document.getElementById("speedComparisonNote");

    const simulateBtn   = document.getElementById("simulateBtn");
    const logEl         = document.getElementById("battleLogBox");
    const gauntletInfoEl = document.getElementById("gauntletInfo");
    const progressMagearnaEl = document.getElementById("progressMagearna");
    const progressLandoEl = document.getElementById("progressLando");
    const checkMagearnaEl = document.getElementById("checkMagearna");
    const checkLandoEl = document.getElementById("checkLando");
    const subMagearnaEl = document.getElementById("subMagearna");
    const subLandoEl = document.getElementById("subLando");
    const shareBtn = document.getElementById("shareBtn");
    const shareNoteEl = document.getElementById("shareNote");

    const playerSpriteEl   = document.getElementById("playerSprite");
    const opponentSpriteEl = document.getElementById("opponentSprite");

    const playerHPFillEl = document.getElementById("playerHPFill");
    const opponentHPFillEl = document.getElementById("opponentHPFill");
    const playerHPTextEl = document.getElementById("playerHPText");
    const opponentHPTextEl = document.getElementById("opponentHPText");
    const playerHPNameEl = document.getElementById("playerHPName");
    const opponentHPNameEl = document.getElementById("opponentHPName");

    const STORAGE_KEY = "gauntlet_progress_v2";
    const NAME_KEY = "gauntlet_player_name_v1";
    const BUILD_KEY = "gauntlet_builds_v1";

    function loadProgress() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return { attemptsMagearna: 0, attemptsLando: 0, firstWinMagearna: null, firstWinLando: null };
        const data = JSON.parse(raw);
        return {
          attemptsMagearna: Number.isFinite(data.attemptsMagearna) ? data.attemptsMagearna : 0,
          attemptsLando: Number.isFinite(data.attemptsLando) ? data.attemptsLando : 0,
          firstWinMagearna: (data.firstWinMagearna === null || Number.isFinite(data.firstWinMagearna)) ? data.firstWinMagearna : null,
          firstWinLando: (data.firstWinLando === null || Number.isFinite(data.firstWinLando)) ? data.firstWinLando : null
        };
      } catch {
        return { attemptsMagearna: 0, attemptsLando: 0, firstWinMagearna: null, firstWinLando: null };
      }
    }
    function saveProgress() {
      const payload = { attemptsMagearna, attemptsLando, firstWinMagearna, firstWinLando };
      localStorage.setItem(STORAGE_KEY, JSON.stringify(payload));
    }

    function loadBuilds() {
      try {
        const raw = localStorage.getItem(BUILD_KEY);
        if (!raw) return { magearna: null, lando: null };
        const data = JSON.parse(raw);
        return { magearna: data.magearna || null, lando: data.lando || null };
      } catch { return { magearna: null, lando: null }; }
    }
    function saveBuilds() {
      localStorage.setItem(BUILD_KEY, JSON.stringify({ magearna: clearedBuildMagearna, lando: clearedBuildLando }));
    }

    function loadName() { return (localStorage.getItem(NAME_KEY) || "").trim(); }
    function saveName(name) { localStorage.setItem(NAME_KEY, (name || "").trim()); }
    function isNameValid() { return playerNameEl.value.trim().length > 0; }
    function updateSimulateEnabled() { simulateBtn.disabled = !isNameValid(); }

    let { attemptsMagearna, attemptsLando, firstWinMagearna, firstWinLando } = loadProgress();
    let { magearna: clearedBuildMagearna, lando: clearedBuildLando } = loadBuilds();

    function updateGauntletInfo() {
      const magearnaCleared = firstWinMagearna !== null;
      const landoCleared = firstWinLando !== null;

      progressMagearnaEl.classList.toggle("progress-done", magearnaCleared);
      progressMagearnaEl.classList.toggle("progress-pending", !magearnaCleared);
      checkMagearnaEl.textContent = magearnaCleared ? "✅" : "⬜";
      subMagearnaEl.textContent = magearnaCleared
        ? `Cleared on attempt #${firstWinMagearna}.`
        : `Attempts so far: ${attemptsMagearna}.`;

      progressLandoEl.classList.toggle("progress-done", landoCleared);
      progressLandoEl.classList.toggle("progress-pending", !landoCleared);
      checkLandoEl.textContent = landoCleared ? "✅" : "⬜";
      subLandoEl.textContent = landoCleared
        ? `Cleared on attempt #${firstWinLando}.`
        : `Attempts so far: ${attemptsLando}.`;

      if (magearnaCleared && landoCleared) {
        gauntletInfoEl.textContent = '✅ Gauntlet complete! Please click "SHARE RESULTS" and share the copied picture with Kevin.';
        shareBtn.disabled = false;
      } else if (magearnaCleared || landoCleared) {
        gauntletInfoEl.textContent = "You cleared one gauntlet. You may continue for the second, or click SHARE RESULTS to give up and submit your current result.";
        shareBtn.disabled = false;
      } else {
        gauntletInfoEl.textContent = "You must clear BOTH gauntlets: win once as Magearna and once as Landorus-Therian.";
        shareBtn.disabled = true;
      }
    }

    function populateItemDropdown() {
      const pokemonId = playerPokemonEl.value;
      const pool = itemsByPokemon[pokemonId];
      playerItemEl.innerHTML = "";
      pool.forEach(it => {
        const opt = document.createElement("option");
        opt.value = it.id;
        opt.textContent = it.name;
        playerItemEl.appendChild(opt);
      });
      updateItemInfo();
    }
    function updateItemInfo() {
      const pokemonId = playerPokemonEl.value;
      const itemId = playerItemEl.value;
      const pool = itemsByPokemon[pokemonId];
      const item = pool.find(i => i.id === itemId);
      itemInfoEl.textContent = item ? item.description : "";
    }

    function populateNatureDropdown() {
      const pokemonId = playerPokemonEl.value;
      const options = natureOptionsFor(pokemonId);
      playerNatureEl.innerHTML = "";
      const statNamesLong = { atk: "Attack", def: "Defense", spa: "Special Attack", spd: "Special Defense", spe: "Speed" };
      options.forEach(n => {
        const opt = document.createElement("option");
        opt.value = n;
        const nat = natures[n];
        if (!nat.plus || !nat.minus) opt.textContent = `${n} (Neutral)`;
        else {
          const plusStat = statNamesLong[nat.plus] || nat.plus;
          const minusStat = statNamesLong[nat.minus] || nat.minus;
          opt.textContent = `${n} (+10% ${plusStat}, -10% ${minusStat})`;
        }
        playerNatureEl.appendChild(opt);
      });
    }

    function populateMoveDropdownAndTable() {
      const pokemonId = playerPokemonEl.value;
      const pData = pokemonData[pokemonId];
      playerMoveEl.innerHTML = "";
      playerMovesTableBody.innerHTML = "";

      const itemId = playerItemEl.value;

      pData.moves.forEach(m => {
        if (m.zMoveOf && m.zMoveOf === "flashcannon" && itemId !== "steeliumz") return;
        if (m.zMoveOf && m.zMoveOf === "fly" && itemId !== "flyiniumz") return;

        const opt = document.createElement("option");
        opt.value = m.id;
        opt.textContent = m.name;
        playerMoveEl.appendChild(opt);

        const tr = document.createElement("tr");
        const stab = pData.types.includes(m.type) ? "Yes" : "No";
        tr.innerHTML = `<td>${m.name}</td><td>${m.type}</td><td>${m.category}</td><td>${m.power}</td><td>${stab}</td>`;
        playerMovesTableBody.appendChild(tr);
      });
    }

    function getEVInputs() {
      return {
        hp:  +evInputs.hp.value  || 0,
        atk: +evInputs.atk.value || 0,
        def: +evInputs.def.value || 0,
        spa: +evInputs.spa.value || 0,
        spd: +evInputs.spd.value || 0,
        spe: +evInputs.spe.value || 0
      };
    }

    const EV_CAP = 510;
    function enforceEvCap(changedKey) {
      const ev = getEVInputs();
      const total = ev.hp + ev.atk + ev.def + ev.spa + ev.spd + ev.spe;
      if (total <= EV_CAP) return false;

      const over = total - EV_CAP;
      const inp = evInputs[changedKey];
      const currentVal = +inp.value;
      inp.value = Math.max(0, currentVal - over);
      return true;
    }

    function updateEVLabelsAndNote(capped=false) {
      const ev = getEVInputs();
      evVals.hp.textContent  = ev.hp;
      evVals.atk.textContent = ev.atk;
      evVals.def.textContent = ev.def;
      evVals.spa.textContent = ev.spa;
      evVals.spd.textContent = ev.spd;
      evVals.spe.textContent = ev.spe;

      const total = ev.hp + ev.atk + ev.def + ev.spa + ev.spd + ev.spe;
      evTotalNoteEl.textContent = `Total EVs Used: ${total}/${EV_CAP}.`;
      evTotalNoteEl.classList.toggle("highlight", total === EV_CAP);

      if (total === EV_CAP && capped) {
        evTotalNoteEl.classList.remove("ev-cap-hit");
        void evTotalNoteEl.offsetWidth;
        evTotalNoteEl.classList.add("ev-cap-hit");
      }
    }

    function renderStatsTable(tbodyEl, stats) {
      tbodyEl.innerHTML = "";
      const entries = [
        ["HP", stats.hp],
        ["Attack", stats.atk],
        ["Defense", stats.def],
        ["Special Attack", stats.spa],
        ["Special Defense", stats.spd],
        ["Speed", stats.spe]
      ];
      for (const [label, val] of entries) {
        const tr = document.createElement("tr");
        tr.innerHTML = `<td>${label}</td><td>${val}</td>`;
        tbodyEl.appendChild(tr);
      }
    }

    function getOpponentDataForCurrentPlayer() {
      const playerId = playerPokemonEl.value;
      const opponentId = (playerId === "magearna") ? "landorus-t" : "magearna";
      return { opponentId, preset: opponentPresets[opponentId] };
    }

    function updateSprites(playerId, opponentId) {
      playerSpriteEl.src = spritePaths[playerId];
      playerSpriteEl.alt = pokemonData[playerId].name;
      opponentSpriteEl.src = spritePaths[opponentId];
      opponentSpriteEl.alt = pokemonData[opponentId].name;
    }

    function setHPColor(fillEl, ratio) {
      if (ratio > 0.5) fillEl.style.background = "#22c55e";
      else if (ratio > 0.2) fillEl.style.background = "#eab308";
      else fillEl.style.background = "#ef4444";
    }

    function updateHPUI(player, opponent, immediate=false, durationMs=null) {
      const pRatio = player.maxHP > 0 ? player.hp / player.maxHP : 0;
      const oRatio = opponent.maxHP > 0 ? opponent.hp / opponent.maxHP : 0;

      setHPColor(playerHPFillEl, pRatio);
      setHPColor(opponentHPFillEl, oRatio);

      const dur = immediate ? 0 : (durationMs ?? 600);
      playerHPFillEl.style.transition = immediate ? "none" : `width ${dur}ms ease`;
      opponentHPFillEl.style.transition = immediate ? "none" : `width ${dur}ms ease`;

      playerHPFillEl.style.width = `${Math.max(0, Math.min(1, pRatio))*100}%`;
      opponentHPFillEl.style.width = `${Math.max(0, Math.min(1, oRatio))*100}%`;

      if (immediate) {
        playerHPTextEl.textContent = `${player.hp} / ${player.maxHP} HP`;
        opponentHPTextEl.textContent = `${opponent.hp} / ${opponent.maxHP} HP`;
      }
    }

    function animateHPText(textEl, startHP, endHP, maxHP, durationMs) {
      if (durationMs <= 0 || startHP === endHP) {
        textEl.textContent = `${endHP} / ${maxHP} HP`;
        return;
      }
      const startTime = performance.now();
      const delta = endHP - startHP;

      function step(now) {
        const t = Math.min(1, (now - startTime) / durationMs);
        const current = Math.round(startHP + delta * t);
        textEl.textContent = `${current} / ${maxHP} HP`;
        if (t < 1) requestAnimationFrame(step);
      }
      requestAnimationFrame(step);
    }

    function updateLivePanels() {
      updateEVLabelsAndNote();

      const playerId = playerPokemonEl.value;
      const ev = getEVInputs();
      const playerStats = buildStatsFor(playerId, playerNatureEl.value, ev, playerItemEl.value);

      const { opponentId, preset } = getOpponentDataForCurrentPlayer();
      const opponentStats = buildStatsFor(opponentId, preset.nature, preset.evs, preset.itemId);

      updateSprites(playerId, opponentId);

      const pool = itemsByPokemon[playerId];
      const itemName = (pool.find(i => i.id === playerItemEl.value) || {}).name || "None";
      playerSummaryEl.textContent =
        `${playerStats.name} (${playerStats.types.join(" / ")}) – Nature: ${playerNatureEl.value}, Item: ${itemName}`;
      opponentSummaryEl.textContent =
        `${opponentStats.name} (${opponentStats.types.join(" / ")}) – Fixed gauntlet build.`;

      renderStatsTable(playerStatsBody, playerStats);
      renderStatsTable(opponentStatsBody, opponentStats);

      playerHPNameEl.textContent = `Your ${playerStats.name}`;
      opponentHPNameEl.textContent = `Opponent ${opponentStats.name}`;
      updateHPUI(playerStats, opponentStats, true);

      playerSpriteEl.classList.toggle("fainted", playerStats.hp <= 0);
      opponentSpriteEl.classList.toggle("fainted", opponentStats.hp <= 0);

      const whoFirst =
        playerStats.spe > opponentStats.spe ? "You" :
        playerStats.spe < opponentStats.spe ? "Opponent" :
        "Speed tie (you move first here)";
      speedComparisonNoteEl.textContent =
        `Your Speed: ${playerStats.spe} | Opponent Speed: ${opponentStats.spe} → ${whoFirst} will move first in this battle.`;
    }

    function getMoveByIdForPokemon(pokemonId, moveId, isOpponent=false) {
      const pData = pokemonData[pokemonId];
      const arr = isOpponent ? pData.opponentMoves : pData.moves;
      return arr.find(m => m.id === moveId);
    }

    function applyDefensiveItems(defender, move, defenderItemId, originalDamage) {
      let damage = originalDamage;
      const defEff = getItemDefensiveEffects(defenderItemId);
      const info = { yacheTriggered: false, focusSashTriggered: false };

      if (defEff.berryType) {
        const isSE = getTypeEffectiveness(move.type, defender.types) > 1;
        if (isSE && move.type === defEff.berryType) {
          damage = Math.floor(damage / 2);
          if (defender.id === "landorus-t" && move.id === "icebeam") {
            info.yacheTriggered = true;
          }
        }
      }

      if (defEff.focusSash && defender.hp === defender.maxHP && damage >= defender.hp) {
        damage = defender.hp - 1;
        if (defender.id === "landorus-t") {
          info.focusSashTriggered = true;
        }
      }

      return { damage, info };
    }

    function triggerAttackAnimation(attackerIsOpponent) {
      const attackerSprite = attackerIsOpponent ? opponentSpriteEl : playerSpriteEl;
      const defenderSprite = attackerIsOpponent ? playerSpriteEl : opponentSpriteEl;

      [attackerSprite, defenderSprite].forEach(spr => {
        spr.classList.remove(
          "sprite-attack-left",
          "sprite-attack-right",
          "hit-flash",
          "defender-shake"
        );
      });

      void attackerSprite.offsetWidth;
      void defenderSprite.offsetWidth;

      attackerSprite.classList.add(attackerIsOpponent ? "sprite-attack-right" : "sprite-attack-left");
      defenderSprite.classList.add("hit-flash", "defender-shake");
    }

    function sleep(ms){ return new Promise(res=>setTimeout(res, ms)); }

    function snapshotCurrentBuild(pokemonId) {
      const ev = getEVInputs();
      const pool = itemsByPokemon[pokemonId];
      const item = pool.find(i => i.id === playerItemEl.value);
      return {
        evs: { ...ev },
        nature: playerNatureEl.value,
        itemId: playerItemEl.value,
        itemName: item ? item.name : playerItemEl.value
      };
    }

    const HP_PER_SECOND = 75;

    async function runSimulation() {
      if (!isNameValid()) {
        alert("Please enter your Discord name / tag before battling.");
        playerNameEl.focus();
        updateSimulateEnabled();
        return;
      }

      simulateBtn.disabled = true;

      const playerId = playerPokemonEl.value;
      if (playerId === "magearna") attemptsMagearna++;
      else attemptsLando++;
      saveProgress();

      const player = buildStatsFor(playerId, playerNatureEl.value, getEVInputs(), playerItemEl.value);
      const { opponentId, preset } = getOpponentDataForCurrentPlayer();
      const opponent = buildStatsFor(opponentId, preset.nature, preset.evs, preset.itemId);

      const playerMove = getMoveByIdForPokemon(playerId, playerMoveEl.value, false);
      const opponentMove = getMoveByIdForPokemon(opponentId, preset.willUseMoveId, true);

      updateHPUI(player, opponent, true);
      playerSpriteEl.classList.remove("fainted");
      opponentSpriteEl.classList.remove("fainted");

      [playerSpriteEl, opponentSpriteEl].forEach(el => {
        el.classList.remove(
          "sprite-attack-left",
          "sprite-attack-right",
          "hit-flash",
          "defender-shake"
        );
      });
      void playerSpriteEl.offsetWidth;
      void opponentSpriteEl.offsetWidth;

      const playerFirst = player.spe >= opponent.spe;

      let log = "";
      logEl.textContent = "";
      async function pushLog(lines, delayMs=1500) {
        for (const line of lines) {
          log += line + "\n";
          logEl.textContent = log;
          logEl.scrollTop = logEl.scrollHeight;
          await sleep(delayMs);
        }
      }

      async function attack(attacker, defender, move, attackerItemId, defenderItemId, attackerLabel, defenderLabel) {
        let { damage, typeMult } = calcDamage({ level: LEVEL, attacker, defender, move, attackerItemId });

        if (attacker.id === "magearna" && move.id === "thunderbolt" && defender.id === "landorus-t") {
          damage = 0;
          typeMult = 0;
        }

        const isOpp = attacker.id !== player.id;
        const shownMoveName = isOpp ? "<    >" : move.name;

        triggerAttackAnimation(isOpp);
        await sleep(120);

        const applied = applyDefensiveItems(defender, move, defenderItemId, damage);
        damage = applied.damage;
        const info = applied.info;

        await pushLog([`${attackerLabel} ${attacker.name} used ${shownMoveName} move!`]);

        const preHP = defender.hp;
        defender.hp = Math.max(0, defender.hp - damage);
        const postHP = defender.hp;
        const lost = preHP - postHP;
        const durationMs = Math.max(250, (lost / HP_PER_SECOND) * 1000);

        updateHPUI(player, opponent, false, durationMs);

        if (defenderLabel.startsWith("Your")) {
          animateHPText(playerHPTextEl, preHP, postHP, defender.maxHP, durationMs);
        } else {
          animateHPText(opponentHPTextEl, preHP, postHP, defender.maxHP, durationMs);
        }

        const animStart = performance.now();

        const midLines = [];
        if (typeMult === 0) {
          midLines.push(`It had no effect...`);
        } else {
          if (typeMult > 1) midLines.push(`It's super effective!`);
          if (typeMult < 1) midLines.push(`It's not very effective...`);
        }

        if (info.yacheTriggered) {
          midLines.push(`(Landorus-Therian ate its Yache Berry!)`);
          midLines.push(`The Yache Berry weakened the damage to Landorus-Therian!`);
        }

        if (midLines.length) await pushLog(midLines);

        const elapsed = performance.now() - animStart;
        const remaining = durationMs - elapsed;
        if (remaining > 0) await sleep(remaining + 50);

        const postLines = [];
        postLines.push(`${defenderLabel} ${defender.name} took ${damage} damage (HP: ${defender.hp}/${defender.maxHP}).`);

        if (info.focusSashTriggered) {
          postLines.push(`Landorus-Therian hung on using its Focus Sash!`);
        }

        await pushLog(postLines);

        if (defender.hp <= 0) {
          const defenderSprite = defenderLabel.startsWith("Your") ? playerSpriteEl : opponentSpriteEl;
          defenderSprite.classList.add("fainted");
          await pushLog([`${defenderLabel} ${defender.name} fainted!`]);
        }
      }

      if (playerFirst) {
        await attack(player, opponent, playerMove, playerItemEl.value, preset.itemId, "Your", "Opponent's");
        if (opponent.hp > 0) {
          await attack(opponent, player, opponentMove, preset.itemId, playerItemEl.value, "Opponent's", "Your");
        }
      } else {
        await attack(opponent, player, opponentMove, preset.itemId, playerItemEl.value, "Opponent's", "Your");
        if (player.hp > 0) {
          await attack(player, opponent, playerMove, playerItemEl.value, preset.itemId, "Your", "Opponent's");
        }
      }

      playerSpriteEl.classList.toggle("fainted", player.hp <= 0);
      opponentSpriteEl.classList.toggle("fainted", opponent.hp <= 0);

      await pushLog(["", "=== Outcome ==="]);

      let playerWin = false;
      if (opponent.hp <= 0 && player.hp > 0) {
        await pushLog(["You WIN this battle! The opposing Pokémon was KO'd."]);
        playerWin = true;
      } else {
        await pushLog(["You LOSE. You failed to faint the opponent within 1 turn."]);
      }

      if (playerWin) {
        if (playerId === "magearna") {
          if (firstWinMagearna === null) firstWinMagearna = attemptsMagearna;
          clearedBuildMagearna = snapshotCurrentBuild("magearna");
        }
        if (playerId === "landorus-t") {
          if (firstWinLando === null) firstWinLando = attemptsLando;
          clearedBuildLando = snapshotCurrentBuild("landorus-t");
        }
        saveProgress();
        saveBuilds();
      }

      updateGauntletInfo();
      updateSimulateEnabled();
      simulateBtn.disabled = !isNameValid();
    }

    function setDefaultEVsForPokemon() {
      evInputs.hp.value = 0;
      evInputs.atk.value = 0;
      evInputs.def.value = 0;
      evInputs.spa.value = 0;
      evInputs.spd.value = 0;
      evInputs.spe.value = 0;
      updateEVLabelsAndNote();
    }

    function applyDefaultItemAndNature(pokemonId) {
      populateNatureDropdown();
      playerNatureEl.value = "Docile";

      populateItemDropdown();
      playerItemEl.value = "none";

      updateItemInfo();
      populateMoveDropdownAndTable();
    }

    function onPlayerPokemonChange() {
      const pokemonId = playerPokemonEl.value;
      applyDefaultItemAndNature(pokemonId);
      setDefaultEVsForPokemon();
      updateLivePanels();
    }

    playerPokemonEl.addEventListener("change", onPlayerPokemonChange);
    playerNatureEl.addEventListener("change", updateLivePanels);
    playerItemEl.addEventListener("change", () => {
      updateItemInfo();
      populateMoveDropdownAndTable();
      updateLivePanels();
    });
    playerMoveEl.addEventListener("change", updateLivePanels);

    Object.entries(evInputs).forEach(([key, inp]) => {
      inp.addEventListener("input", () => {
        const capped = enforceEvCap(key);
        updateEVLabelsAndNote(capped);
        updateLivePanels();
      });
    });

    simulateBtn.addEventListener("click", runSimulation);

    playerNameEl.addEventListener("input", () => {
      saveName(playerNameEl.value);
      updateSimulateEnabled();
    });

    function fmtEvs(evs) {
      if (!evs) return "—";
      return `${evs.hp}/${evs.atk}/${evs.def}/${evs.spa}/${evs.spd}/${evs.spe}`;
    }

    async function generateShareImageBlob() {
      const width = 760, height = 520;
      const canvas = document.createElement("canvas");
      canvas.width = width; canvas.height = height;
      const ctx = canvas.getContext("2d");
      const playerName = (playerNameEl.value || "").trim() || "Unknown";

      ctx.fillStyle = "#f8fafc";
      ctx.fillRect(0,0,width,height);

      ctx.strokeStyle = "#94a3b8";
      ctx.lineWidth = 4;
      ctx.strokeRect(10,10,width-20,height-20);

      ctx.fillStyle = "#0f172a";
      ctx.font = "bold 28px Arial";
      ctx.fillText("Magearna & Landorus-Therian Gauntlet", 40, 60);

      ctx.font = "bold 20px Arial";
      ctx.fillText(`Player: ${playerName}`, 40, 95);

      const magearnaCleared = firstWinMagearna !== null;
      const landoCleared = firstWinLando !== null;
      const gauntletComplete = magearnaCleared && landoCleared;

      ctx.font = "bold 20px Arial";
      ctx.fillStyle = magearnaCleared ? "#16a34a" : "#b91c1c";
      ctx.fillText(
        `Magearna: ${magearnaCleared ? "CLEARED" : "NOT COMPLETED"}${magearnaCleared ? ` on attempt #${firstWinMagearna}` : ""}`,
        40, 150
      );
      if (gauntletComplete) {
        ctx.fillStyle = "#0f172a";
        ctx.font = "18px Arial";
        ctx.fillText(
          `Build — EVs: ${fmtEvs(clearedBuildMagearna?.evs)} | Nature: ${clearedBuildMagearna?.nature || "—"} | Item: ${clearedBuildMagearna?.itemName || "—"}`,
          60, 180
        );
      }

      ctx.font = "bold 20px Arial";
      ctx.fillStyle = landoCleared ? "#16a34a" : "#b91c1c";
      ctx.fillText(
        `Landorus-T: ${landoCleared ? "CLEARED" : "NOT COMPLETED"}${landoCleared ? ` on attempt #${firstWinLando}` : ""}`,
        40, 235
      );
      if (gauntletComplete) {
        ctx.fillStyle = "#0f172a";
        ctx.font = "18px Arial";
        ctx.fillText(
          `Build — EVs: ${fmtEvs(clearedBuildLando?.evs)} | Nature: ${clearedBuildLando?.nature || "—"} | Item: ${clearedBuildLando?.itemName || "—"}`,
          60, 265
        );
      }

      ctx.fillStyle = "#0f172a";
      ctx.font = "18px Arial";
      ctx.fillText("DM Kevin your results image.", 40, 325);

      const ts = new Date().toLocaleString();
      ctx.font = "14px Arial";
      ctx.fillStyle = "#64748b";
      ctx.fillText(ts, 40, 380);

      return await new Promise(resolve => canvas.toBlob(resolve, "image/png"));
    }

    shareBtn.addEventListener("click", async () => {
      if (firstWinMagearna === null && firstWinLando === null) return;
      shareNoteEl.textContent = "";
      try {
        const blob = await generateShareImageBlob();
        if (navigator.clipboard && window.ClipboardItem) {
          await navigator.clipboard.write([new ClipboardItem({ "image/png": blob })]);
          shareNoteEl.textContent = "Copied an image to your clipboard! and DM Kevin (don't post in Giveaway Group)";
        } else {
          const url = URL.createObjectURL(blob);
          const a = document.createElement("a");
          a.href = url;
          a.download = "gauntlet-result.png";
          document.body.appendChild(a);
          a.click();
          a.remove();
          URL.revokeObjectURL(url);
          shareNoteEl.textContent = "Your browser can't copy images directly, so a download started instead. Please DM Kevin the image.";
        }
      } catch (e) {
        shareNoteEl.textContent = "Could not copy image. Try a different browser (Chrome/Edge works best).";
      }
    });

    (function init() {
      applyDefaultItemAndNature(playerPokemonEl.value);
      setDefaultEVsForPokemon();
      populateMoveDropdownAndTable();
      updateLivePanels();
      updateGauntletInfo();

      playerNameEl.value = loadName();
      updateSimulateEnabled();
    })();

</script>
