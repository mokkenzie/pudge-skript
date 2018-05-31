-- Повозиться.lua для зонтика
-- fl0wstream вит NBOMe, Май 2018
-- Последнее обновление: 27/05/2018

-- Утилиты.луа по Эроика-ЧГК
местные коммунальные = требуют("скрипты/Эроика/Полезность")

местные Тинкер = {}

-- Меню рисовые шарики
Тинкер.menuToggle = меню.AddOptionBool({"герой конкретные", "Тинкер"}, "включен", ложь)
Тинкер.checkForSafeCast = меню.AddOptionBool({"герой конкретные", "Тинкер"}, "безопасный Литой", правда)
Тинкер.drawDamage = меню.AddOptionBool({"герой конкретные", "Тинкер"}, "обратить вред", правда)
--Повозиться.drawDamageSize = меню.AddOptionSlider({"Конкретного Героя", "Динь-Динь"}, "Нарисуй Размер Ущерба", 8, 24, 14)

-- > Врага Селектор Настройки
--Повозиться.enemySelectorMode = меню.AddOptionCombo({"Герой Удельная", "Лудильщик", "Враг Селектор"}, "Враг Переключатель Режимов", {"Простой", "Умный"}, 0)
Тинкер.enemyMouseRange = меню.AddOptionSlider({"герой конкретные", "Лудильщик", "враг "селектор""}, "мышь Диапазон поиск", 100, 2000, 1000)
Тинкер.selectorAwareBlademail = меню.AddOptionBool({"герой конкретные", "Лудильщик", "враг селектор"}, "игнорировать врагов с лезвием почте или Lotus", ложь)

-- Меню ключи
Тинкер.menuComboKey = меню.AddKeyOption({"Герой Конкретные", "Тинкер"}, "Ключ Комбинированный", Перечисление.ButtonCode.KEY_G)

-- Пункты меню
Тинкер.меню = {ItemSoulring = "душа кольцо",
 ItemHex = "косы тиски",
 ItemDagon = "Дагон",
 ItemEthereal = "Эфирный Клинок",
 ItemVeil = "вуаль раздора",
 ItemBlink = "Блинк Даггер",
 ItemOrchid = "Орхидея Злорадства",
 ItemShiva = "Шивы гвардии"}
                            --ItemEuls = "Euls"}

Тинкер.menuItemsHandle = {ItemSoulring,
 ItemHex,
 ItemDagon,
 ItemEthereal,
 ItemVeil,
 ItemBlink,
 ItemOrchid,
 ItemShiva}
                            --ItemEuls}
                            

для К В в парах(Тинкер.меню) сделать
 Тинкер.menuItemsHandle[к] = меню.AddOptionBool({"герой конкретные", "Лудильщик", "использование предметов"}, Тинкер.меню[к], правда)
конец

-- Меню способностей
Тинкер.menuAbilities = {SpellLaser = "лазер",
 SpellRockets = "Ракет",
 SpellMarch = "Марта",
 SpellRefresh = "Обновить"}

Тинкер.menuAbilitiesHandle = {SpellLaser,
 SpellRockets,
 SpellMarch,
 SpellRefresh}

для К В в парах(Тинкер.menuAbilities) делать
 Тинкер.menuAbilitiesHandle[к] = меню.AddOptionBool({"герой конкретные", "Лудильщик", "возможности использования"}, Тинкер.menuAbilities[к], правда)
конец

-- Меню linkens попсовое'
Тинкер.menuLinkensPoppers = {ItemHex = "косы тиски",
 ItemDagon = "Дагон",
 ItemEthereal = "Эфирный Клинок",
 ItemOrchid = "Орхидея Злорадства",
 ItemEuls = "Euls",
 SpellLaser = "Заклинание: Лазерный"}

Тинкер.menuLinkensHandle = {ItemHex,
 ItemDagon,
 ItemEthereal,
 ItemOrchid,
 ItemEuls,
 SpellLaser}

для К В в парах(Тинкер.menuLinkensPoppers) делать
 Тинкер.menuLinkensHandle[к] = меню.AddOptionBool({"герой конкретные", "Лудильщик", "Linkens попперс"}, Тинкер.menuLinkensPoppers[к], ложные)
конец

-- Глобалс
местные CurrentVictim = шь
местные MyHero = шь
местного игрока = шь

местные SpellLaser = шь
местные SpellRockets = шь
местные SpellMarch = шь
местные SpellRefresh = шь

местные ItemSoulring = шь
местные ItemHex = шь
местные ItemDagon = шь
местные ItemEthereal = шь
местные ItemVeil = шь
местные ItemBlink = шь
местные ItemOrchid = шь
местные ItemShiva = шь
местные ItemEuls = шь

местные NextNukeTime = 0
местные NextRefreshTime = 0

местные AbilityChannelTime = {3.0, 1.5, 0.75}

местные шрифта = Рендерер.LoadFont("Тахома", 16, Перечислимый.Свойство fontweight.Жирным шрифтом) 

-- Функции
-- OnGameStart() - сброс всех местных жителей
функция повозиться.OnGameStart()
 CurrentVictim = шь

 MyHero = шь
 Игрока = шь

 Журнал.Писать("Динь > OnGameStart() -> сброс сделал")
конец

-- OnGameEnd() - то же, что старт игры
функция повозиться.OnGameEnd()
 CurrentVictim = шь

 MyHero = шь
 Игрока = шь

 SpellLaser = шь
 SpellRockets = шь
 SpellRefresh = шь
 SpellMarch = шь

 ItemSoulring = шь
 ItemHex = шь
 ItemDagon = шь
 ItemEthereal = шь
 ItemVeil = шь
 ItemBlink = шь
 ItemOrchid = шь
 ItemShiva = шь
 ItemEuls = шь

 Журнал.Писать("Динь > OnGameEnd() -> сброс сделал")
конец

-- OnUpdate() - обновляет этот тинкер правда говно каждом тике работать
функция повозиться.OnUpdate()
    -- проверка мы бля Катька
    если не Двигатель.IsInGame() 
        или герои.GetLocal() == ноль
        или не GameRules.GetGameState() == 5
        или нет меню.Свойства Isenabled(Тинкер.menuToggle)
        или GameRules.IsPaused() тогда
            вернуться
    конец

    -- или, может быть, мы не существуем и Плайя' до тха гребаный глоб@общ.
    если MyHero == шь тогда MyHero = герои.GetLocal() конец
    если игрока == шь то игрока = игроки.GetLocal() конец

    -- или, может быть, мы не халтурили хD -- или, может быть, мы умерли
    если НЕПИСЬ.GetUnitName(MyHero) ~= "npc_dota_hero_tinker" или не лицо.Потока isalive(MyHero) потом возвращение конец

    -- принеси наши способности и предметы, Ю
 Тинкер.FetchAbilities()
 Тинкер.FetchItems()

    -- враг выборе
 CurrentVictim = шь
 Тинкер.EnemySelector()

    -- Комбинированная часть, только пока keypressed
    если меню.IsKeyDown(Тинкер.menuComboKey) тогда
        если CurrentVictim потом повозиться.DoCombo() конец
    конец
конец

функция повозиться.Функцию ondraw()
    если нет меню.Свойства Isenabled(Тинкер.drawDamage) затем вернуться конце

    если не MyHero затем вернуться конце
    если НЕПИСЬ.GetUnitName(MyHero) ~= "npc_dota_hero_tinker" после возвращения конца

    для я = 1, герои.Граф() сделать
        местный герой = герои.Вам(мне)
        
        если не сущность.IsSameTeam(MyHero, герой) , а не сущности.IsDormant(герой) , а не НПЦ.IsIllusion(героя) и субъекта.Потока isalive(герой) тогда
            местные поз = сущность.GetAbsOrigin(герой)
 пос:Сэты(поз:Гети() + 50)

            местные х, Г, ВИС = рендер.WorldToScreen(поз)

            если бы не ВИС , то возвращение конец
            
            местные повреждения = повозиться.CalculateDamage(герой)

            местные Дельта = сущность.GetHealth(герой) - повреждение

            если Дельта < 0 тогда Рендерер.SetDrawColor(0, 255, 0, 255) другой Рендерер.SetDrawColor(255, 0, 0, 255) конец

 Рендерер.То drawtext(шрифт, х, Г, математика.пол(Дельта))
        конец
    конец
конец

-- CalculateDamage(героя) - рассчитать ущерб, который может справиться с этим врагом, используя комбо
функция повозиться.CalculateDamage(герой)
    местные pure_damage = 0.0
    местные magic_damage = 0.0

    -- жестко Бога люлька
    местные dmg_multiplier = 1 + герой.GetIntellectTotal(MyHero) * 0.0008765432

    -- Кая
    -- spell_amp
    если НЕПИСЬ.HasItem(MyHero, "item_kaya") тогда 
        местные Кая = НСП.Метод getitem(MyHero, "item_kaya")
 dmg_multiplier = dmg_multiplier + (возможность.GetLevelSpecialValueFor(кайя, "spell_amp") / 100)
    конец

    -- Модификаторы
    если НЕПИСЬ.HasModifier(MyHero, "modifier_bloodseeker_bloodrage") тогда
 dmg_multiplier = dmg_multiplier + модификатор.GetConstantByIndex(ВСНП.GetModifier(MyHero, "modifier_bloodseeker_bloodrage"), 1) / 100
    конец

    если НЕПИСЬ.HasModifier(герой, "modifier_bloodseeker_bloodrage") тогда
 dmg_multiplier = dmg_multiplier + модификатор.GetConstantByIndex(ВСНП.GetModifier(герой, "modifier_bloodseeker_bloodrage"), 1) / 100
    конец

    если НЕПИСЬ.HasModifier(герой, "modifier_chen_penitence") тогда
 dmg_multiplier = dmg_multiplier + модификатор.GetConstantByIndex(ВСНП.GetModifier(герой, "modifier_chen_penitence"), 1) / 100
    конец
    
	если НЕПИСЬ.HasModifier(герой, "modifier_shadow_demon_soul_catcher") тогда
 dmg_multiplier = dmg_multiplier + модификатор.GetConstantByIndex(ВСНП.GetModifier(герой, "modifier_shadow_demon_soul_catcher"), 0) / 100
	конец

	если НЕПИСЬ.HasModifier(герой, "modifier_slardar_sprint") тогда
 dmg_multiplier = dmg_multiplier + модификатор.GetConstantByIndex(ВСНП.GetModifier(герой, "modifier_slardar_sprint"), 0) / 100
	конец

    -- Орхидея логика странная, так что он должен быть здесь
    --если повозиться.CanUseItem(ItemOrchid, Тинкер.menuItemsHandle["ItemOrchid"], герой) тогда
    -- dmg_multiplier = dmg_multiplier + 0.3
    --конец

    местные magic_damage_multiplier = dmg_multiplier - (1 - НСП.GetMagicalArmorDamageMultiplier(герой))

    -- Множители
    если повозиться.CanUseItem(ItemEthereal, Тинкер.menuItemsHandle["ItemEthereal"], герой) тогда
 magic_damage_multiplier = magic_damage_multiplier + 0.4
 magic_damage = magic_damage + (75 + 2 * герой.GetIntellectTotal(MyHero))
    конец

    если повозиться.CanUseItem(ItemVeil, Тинкер.menuItemsHandle["ItemVeil"], герой) тогда
 magic_damage_multiplier = magic_damage_multiplier + 0.25
    конец

    -- Магический урон
    если повозиться.CanUseItem(ItemDagon, Тинкер.menuItemsHandle["ItemDagon"], герой) тогда
 magic_damage = magic_damage + способность.GetLevelSpecialValueFor(ItemDagon, "ущерб")
    конец

    если повозиться.CanUseItem(ItemShiva, Тинкер.menuItemsHandle["ItemShiva"], герой) тогда
 magic_damage = magic_damage + способность.GetLevelSpecialValueFor(ItemShiva, "blast_damage")
    конец

    если повозиться.CanCastAbility(SpellRockets, Тинкер.menuAbilitiesHandle["SpellRockets"], герой) тогда
 magic_damage = magic_damage + способность.GetLevelSpecialValueFor(SpellRockets, "ущерб")
    конец

    -- Чистый урон
    если повозиться.CanCastAbility(SpellLaser, Тинкер.menuAbilitiesHandle["SpellLaser"], герой) тогда
 pure_damage = pure_damage + способность.GetLevelSpecialValueFor(SpellLaser, "laser_damage")
    конец

    возвращение (pure_damage * dmg_multiplier) + (magic_damage * magic_damage_multiplier)
конец

функция повозиться.CanUseItem(пункт, itemHandle, враг)
    если не товар , то вернуть ложь конец

    если не сущность.IsEntity(номенклатура) тогда возврат ложь конец

    если не полезности.IsSuitableToUseItem(MyHero) или не меню.Свойства isenabled(itemHandle) тогда возврат ложь конец
    если меню.Свойства Isenabled(Тинкер.checkForSafeCast) и не полезности.IsSafeToCast(MyHero, противника, способности.GetDamage(пункт)) тогда возврат ложь конец
    если нет Возможность.IsReady(пункт) или не способность.IsCastable(пункт, способности.GetManaCost(пункт)) тогда возврат ложь конец

    возвращение истинного
конец

функция повозиться.CanCastAbility(пункт, itemHandle, враг)
    если не товар , то вернуть ложь конец

    если не сущность.IsEntity(номенклатура) тогда возврат ложь конец

    если не полезности.IsSuitableToCastSpell(MyHero) или не меню.Свойства isenabled(itemHandle) тогда возврат ложь конец
    если меню.Свойства Isenabled(Тинкер.checkForSafeCast) и не полезности.IsSafeToCast(MyHero, противника, способности.GetDamage(пункт)) тогда возврат ложь конец
    если нет Возможность.IsReady(пункт) или не способность.IsCastable(пункт, способности.GetManaCost(пункт)) тогда возврат ложь конец

    возвращение истинного
конец

-- DoCombo() - делаешь комбо на CurrentVictim
функция повозиться.DoCombo()
    --Войдите.Писать("Тинкер.DoCombo() -> комбо выполняется")

    местные враг = CurrentVictim

    -- в первый, может быть, автоатака?)
    если не НЕПИСЬ.HasState(враг, перечисление.ModifierState.MODIFIER_STATE_ATTACK_IMMUNE) тогда
 Плеер.AttackTarget(игрока, MyHero, враг, ложь)
    конец

    -- душа кольцо, уебок
    если повозиться.UseItem(ItemSoulring, Тинкер.menuItemsHandle["ItemSoulring"]) тогда возврат конец

    -- мигать, это сука
    если повозиться.UseBlink() тогда возврат конец

    -- Я щит :(
    если повозиться.IsAntimageShielded(враг) тогда
        если повозиться.Литейные Свойства(SpellLaser, Тинкер.menuLinkensHandle["SpellLaser"]) тогда возврат конец
        если повозиться.UseItem(ItemDagon, Тинкер.menuLinkensHandle["ItemDagon"]) тогда возврат конец
        если повозиться.UseItem(ItemEthereal, Тинкер.menuLinkensHandle["ItemEthereal"]) тогда возврат конец
        если повозиться.UseItem(ItemEuls, Тинкер.menuLinkensHandle["ItemEuls"]) тогда возврат конец
    конец

    -- попсовое linkens, Ю
    если НЕПИСЬ.IsLinkensProtected(враг) тогда
        если повозиться.UseItem(ItemEuls, Тинкер.menuLinkensHandle["ItemEuls"]) тогда возврат конец
        если повозиться.UseItem(ItemOrchid, Тинкер.menuLinkensHandle["ItemOrchid"]) тогда возврат конец
        если повозиться.UseItem(ItemDagon, Тинкер.menuLinkensHandle["ItemDagon"]) тогда возврат конец
        если повозиться.UseItem(ItemEthereal, Тинкер.menuLinkensHandle["ItemEthereal"]) тогда возврат конец
        если повозиться.Литейные Свойства(SpellLaser, Тинкер.menuLinkensHandle["SpellLaser"]) тогда возврат конец
        если повозиться.UseItem(ItemHex, Тинкер.menuLinkensHandle["ItemHex"]) тогда возврат конец
    конец

    -- может быть, хекс?)
    если не НЕПИСЬ.HasState(враг, перечисление.ModifierState.MODIFIER_STATE_HEXED) и не НПС.HasState(враг, перечисление.ModifierState.MODIFIER_STATE_STUNNED) тогда
        если повозиться.UseItem(ItemHex, Тинкер.menuItemsHandle["ItemHex"]) тогда возврат конец
    конец	

    -- бесплотный
    если не НЕПИСЬ.HasModifier(враг, "modifier_item_ethereal_blade_ethereal") тогда
        если повозиться.UseItem(ItemEthereal, Тинкер.menuItemsHandle["ItemEthereal"]) тогда 
            -- ракетные скорость: 1275
            если НЕПИСЬ.IsEntityInRange(MyHero, противника, способности.GetCastRange(ItemEthereal)) тогда
                местные my_pos = сущность.GetAbsOrigin(MyHero)
                местным расстояние = my_pos:расстояние(сущностей.GetAbsOrigin(врага))

 расстояние = расстояние:Длина2d()

                местные flight_time = расстояние / 1275

 NextNukeTime = GameRules.GetGameTime() + flight_time
                вернуться 
            конец
        конец
    конец

    -- вуаль
    если повозиться.UseItem(ItemVeil, Тинкер.menuItemsHandle["ItemVeil"]) тогда возврат конец

    -- орхидея
    если повозиться.UseItem(ItemOrchid, Тинкер.menuItemsHandle["ItemOrchid"]) тогда возврат конец

    -- Шива
    если повозиться.UseItem(ItemShiva, Тинкер.menuItemsHandle["ItemShiva"]) тогда возврат конец

    -- ждет неземная появляется на поле
    если GameRules.GetGameTime() < NextNukeTime и NextNukeTime ~= 0 тогда возврат конец
 NextNukeTime = 0

    -- дагон
    если повозиться.UseItem(ItemDagon, Тинкер.menuItemsHandle["ItemDagon"]) тогда возврат конец

    -- лазер
    если повозиться.Литейные Свойства(SpellLaser, Тинкер.menuAbilitiesHandle["SpellLaser"]) тогда возврат конец

    -- ракеты
    если повозиться.Литейные Свойства(SpellRockets, Тинкер.menuAbilitiesHandle["SpellRockets"]) тогда возврат конец

    -- жду следующего обновления
    если GameRules.GetGameTime() < NextRefreshTime и NextRefreshTime ~= 0 тогда возврат конец
 NextRefreshTime = 0

    -- обновить может?
    если нет Возможность.IsReady(SpellLaser) и 
    не способность.IsReady(SpellRockets)
    тогда
        если повозиться.Литейные Свойства(SpellRefresh, Тинкер.menuAbilitiesHandle["SpellRefresh"]) тогда 
 NextRefreshTime = GameRules.GetGameTime() + Способность.GetCastPoint(SpellRefresh) + AbilityChannelTime[Способность.GetLevel(SpellRefresh) - 1]
            вернуться 
        конец
    конец
конец

-- IsAntimageShielded(героя) - возвращение героя == Антимайдан и ready2use щит 
функция повозиться.IsAntimageShielded(герой)
    местный щит = НСП.GetAbility(герой, "antimage_spell_shield")
	если щит и способности.IsReady(щит) и НИП.HasItem(герой, "item_ultimate_scepter", истина) тогда
		возвращение истинного
	конец
конец

-- FetchAbilities() - получает способности от наших чумовая повозиться
функция повозиться.FetchAbilities()
 SpellLaser = НСП.GetAbility(MyHero, "tinker_laser")
 SpellRockets = НСП.GetAbility(MyHero, "tinker_heat_seeking_missile")
 SpellMarch = НСП.GetAbility(MyHero, "tinker_march_of_the_machines")
 SpellRefresh = НСП.GetAbility(MyHero, "tinker_rearm")
конец

-- FetchItems() - получает детали от нашего чумовая повозиться
функция повозиться.FetchItems()
 ItemBlink = НСП.Метод getitem(MyHero, "item_blink") 
 ItemDagon = НСП.Метод getitem(MyHero, "item_dagon")
    если не ItemDagon тогда
		для я = 2, 5 сделать
 ItemDagon = НСП.Метод getitem(MyHero, "item_dagon_" .. я, правда)
			если ItemDagon потом перерыв закончится
		конец
    конец
 ItemEthereal = НСП.Метод getitem(MyHero, "item_ethereal_blade")
 ItemHex = НСП.Метод getitem(MyHero, "item_sheepstick")
 ItemOrchid = НСП.Метод getitem(MyHero, "item_orchid")
 ItemShiva = НСП.Метод getitem(MyHero, "item_shivas_guard")
 ItemSoulring = НСП.Метод getitem(MyHero, "item_soul_ring")
 ItemEuls = НСП.Метод getitem(MyHero, "item_cyclone")
 ItemVeil = НСП.Метод getitem(MyHero, "item_veil_of_discord")
конец

-- UseItem(элемент, MenuHandle ручка) - использует товар надлежащим образом (в противника или в землю рядом с ним)
функция повозиться.UseItem(пункт, itemHandle)
    местные враг = CurrentVictim

    -- омг Нил пункты 4 уебков
    если не товар , то вернуть ложь конец

    если не сущность.IsEntity(номенклатура) тогда возврат ложь конец

    если не полезности.IsSuitableToUseItem(MyHero) или не меню.Свойства isenabled(itemHandle) тогда возврат ложь конец
    если меню.Свойства Isenabled(Тинкер.checkForSafeCast) и не полезности.IsSafeToCast(MyHero, противника, способности.GetDamage(пункт)) тогда возврат ложь конец
    если нет Возможность.IsReady(пункт) или не способность.IsCastable(пункт, способности.GetManaCost(пункт)) тогда возврат ложь конец

    местные target_type = способность.GetTargetType(пункт)
    --Войдите.Писать(target_type .. " | " .. возможность.Метод getname(пункт))

    -- нет-цель
    если (target_type == 0 и элемент ~= ItemSoulring и пункт ~= ItemShiva) тогда
 Способность.CastPosition(предмет, существо.GetAbsOrigin(врага))
        возвращение истинного
    конец
    -- герой или все
    если (target_type == 3 или
 target_type == 19 или
 target_type == 128) тогда
 Способность.CastTarget(элемент, враг)
        возвращение истинного
    конец
    
    -- других случаях
 Способность.CastNoTarget(пункт)
    возвращение истинного
конец    

-- Литейные свойства(способность способность, MenuHandle ручка) - использует способность правильно (в противника или в землю рядом с ним)
функция повозиться.Литейные свойства(способности, abilityHandle)
    местные враг = CurrentVictim

    -- Боже шь способности 4 уебков
    если нет возможности потом вернуть значение false конца

    если не сущность.IsEntity(способность) тогда возврат ложь конец

    если не полезности.IsSuitableToCastSpell(MyHero) или не меню.Свойства isenabled(abilityHandle) тогда возврат ложь конец
    если меню.Свойства Isenabled(Тинкер.checkForSafeCast) и не полезности.IsSafeToCast(MyHero, противника, способности.GetDamage(способность)) тогда возврат ложь конец
    если нет Возможность.IsReady(способности) или не способность.IsCastable(способность, умение.GetManaCost(способность)) тогда возврат ложь конец

    местные target_type = способность.GetTargetType(способность)
    --Войдите.Писать(target_type .. " | " .. возможность.Метод getname(способность))

    -- нет-цель
    если (target_type == 0) тогда
 Способность.CastNoTarget(способность)
        возвращение истинного
    конец
    -- герой или все
    если (target_type == 3 или 
 target_type == 19 или
 target_type == 128) тогда
 Способность.CastTarget(способность, враг)
        возвращение истинного
    конец
    
    -- других случаях
 Способность.CastPosition(способности, образование.GetAbsOrigin(врага))
    возвращение истинного
конец    

-- UseBlink() - литой мигать правильно, не в лицо топор, например
функция повозиться.UseBlink()
    если повозиться.CanUseItem(ItemBlink, Тинкер.menuItemsHandle["ItemBlink"], CurrentVictim) тогда
        местные cast_range = способность.GetLevelSpecialValueFor(ItemBlink, "blink_range") + НПС.GetCastRangeBonus(MyHero)

        если НЕПИСЬ.IsEntityInRange(MyHero, CurrentVictim, 650) затем вернуться конце

        местные my_loc = сущность.GetAbsOrigin(MyHero)
        местным расстояние = сущность.GetAbsOrigin(CurrentVictim) - my_loc

 расстояние:Сэтз(0)
 расстояние:нормализовать()
 расстояние:масштаб(cast_range - 1)
 расстояние:масштаб(0.7)

        местные blink_position = my_loc + расстояние

 Плеер.PrepareUnitOrders(Игрока, 
 Перечисление.UnitOrder.DOTA_UNIT_ORDER_CAST_POSITION,
 CurrentVictim,
 blink_position,
 ItemBlink,
 Перечисление.PlayerOrderIssuer.DOTA_ORDER_ISSUER_HERO_ONLY,
 MyHero,
                            ложные,
                            ложные)
        возвращение истинного
    конец
    возвращение ложным
конец

-- EnemySelector(), назвав ее, когда нам нужны некоторые жертвы хаха
функция повозиться.EnemySelector()
    местные selector_mode = 0 --меню.И Getvalue(Тинкер.enemySelectorMode)
    местные mouse_range = меню.Метод Getvalue(Тинкер.enemyMouseRange)
    местные selector_aware_blademail = меню.Свойства Isenabled(Тинкер.selectorAwareBlademail)

    местные temp_enemy = шь

    -- Враг селектор: режим 0 - простой (просто выбрать случайного героя вокруг мыши лол)
    если selector_mode == 0 и не temp_enemy тогда
 temp_enemy = ввод.GetNearestHeroToCursor(Сущностей.GetTeamNum(MyHero), Перечисление.TeamType.TEAM_ENEMY)
        если утилита.CanCastSpellOn(temp_enemy) и temp_enemy ~= шь и ВСНП.IsPositionInRange(temp_enemy, ввод.GetWorldCursorPos(), mouse_range) тогда
            если selector_aware_blademail тогда
                если утилита.IsLotusProtected(temp_enemy) или НПС.HasModifier(temp_enemy, "modifier_item_blade_mail_reflect") тогда
 temp_enemy = шь
                    вернуться
                конец           
            конец
 CurrentVictim = temp_enemy
        конец
    конец

 temp_enemy = шь
конец

возвращение повозиться
