# BF4_Stats_Research

Here i am trying to understand stat values that are sent from blaze server, more specifically the stat values that starts with c_  
Later i may look at other stat values.

# Criteria stat codes

-----------------------------------------------------------  
The criteria stat code format
  1) **c_code_paramX_paramY_statEvent_criteriaType**
  2) **c_paramX_paramY_statEvent_criteriaType**
-----------------------------------------------------------  

**code**, **paramX** and **paramY** can be null, in other words they can be empty string in stat code (yes i know null != "")


# Stat Events

By looking at stat codes (for example "c_aku__shw_g") it is unclear what stat event (in this case "shw"). I took a look at the battlelog apk awards json file (my interest in bf4 stats research appeared when i stumbled upon this file). I understood that these short names are stat events (for example "kwa" => StatEvent_KillWith_X_As_Y), so i looked further to get full stat event list. I searched for these stat events in bf4 ebx files (https://github.com/GreyDynamics/BF4_EBX) and found a place where stat event enum is used (https://github.com/GreyDynamics/BF4_EBX/blob/4e19b479df7294c70744fedf762575bdef9c6db8/Persistence/SpamSettings_offset_.txt). In order to get full stat event list, i needed to dump that particular ebx file, so i did and got the full stat event list (From server version R58). In order to get this stat event list usable i wrote this stat event enum in C#. You can see it down below. Commnets - some descriptions i found for stat events while looking at bf4 ebx files.

```C#
enum StatEvent
{
    Kill,
    Kill_In_X, //Global All Time Kill In X
    Kill_As_X, //Global All Time Kill As X
    Kill_As_HVT_Bonus,
    Kill_On_X, //Highest Number of Kills on Level X
    KillWith_X,
    KillWith_X_As_Y, //Global All Time Kill With X As Y
    KillWith_X_On_Y,
    KillDistance,
    KillOf_X_As_Y,
    KillOf_X_In_Y,
    TeamKill,
    PaybackKill,
    AvengerKill,
    TeamAvengerKill,
    SquadAvengerKill,
    SaviorKill,
    TeamSaviorKill,
    SquadSaviorKill,
    DriverAssist,
    PassengerAssist,
    SquadPassengerAssist,
    RoadKill,
    Death,
    MultiKill,
    MultiKill_On_X, //Highest Number of multi kills on Level X
    PostMortemKill,
    AdrenalineKill,
    AdrenalineKill_On_X, //Highest Number of adrenaline kills on Level X
    KillStreak,
    KillStreakX,
    TimedKillStreak,
    TimedKillStreak_On_X, //Highest Number of timed killstreaks on Level X
    CancelKillStreak,
    Comeback,
    SquadWipe,
    NemesisKill,
    NemesisKillX,
    CancelNemesisKill,
    SecondsAs_X, //Global All Time Seconds As X
    SecondsWith_X, //Global All Time Seconds With X
    SecondsIn_X, //Global All Time Seconds In X
    SecondsOn_X, //Global All Time Seconds On X
    VehicleDamage_X,
    MobilityCriticalHit_X,
    MobilityKill_X,
    FirepowerCriticalHit_X,
    FirepowerKill_X,
    Destroy_X,
    Destroy_X_As_Y,
    Destroy_X_In_Y, //Global All Time Destroy X In Y
    Destroy_X_On_Y,
    DestroyWith_X_As_Y,
    Destroy_X_With_Y,
    DestroyExplosives,
    Disabled_X,
    KillOfWith_X_As_Y,
    TaggedDamageAssist_X,
    SquadTaggedDamageAssist_X,
    TaggedDestroy_X,
    SquadTaggedDestroy_X,
    Tagged_X,
    SuppressWith_X_As_Y,
    Level_X_Completed_Y_Difficulty,
    Level_X_Completed_Y_Difficulty_WithTimeValue,
    CleanLevel_X_Completed,
    RoundOverOutcome_X_On_Y,
    Rounds,
    Headshot_X_With_Y, //Global All Time Headshot X With Y
    HeadshotAs_X_With_Y,
    HeadshotDistance_With_X,
    Headshot_On_X, //Highest Number of headshot kills on Level X
    SelfHeal,
    BestSquad,
    ShotsFired_With_X, //Global All Time Shots Fired With X
    ShotsHit_With_X, //Global All Time Shots Hits With X
    SpotAssist,
    TeamSpotAssist,
    SquadSpotAssist,
    RadarAssist,
    TeamRadarAssist,
    SquadRadarAssist,
    SuppressionAssist,
    SquadSuppressionAssist,
    Heal,
    TeamHeal,
    SquadHeal,
    Revive,
    TeamRevive,
    SquadRevive,
    ReviveAccepted,
    TeamReviveAccepted,
    SquadReviveAccepted,
    ManDownRevive,
    Resupply,
    TeamResupply,
    SquadResupply,
    KillAssist,
    KillAssistAsKill,
    SquadKillAssistAsKill,
    Repair,
    TeamRepair,
    SquadRepair,
    VehicleDestroyAssist,
    TreeDestroyed,
    WallDestroyed,
    CapturingCapturePoint,
    CaptureCapturePoint,
    NeutralizeCapturePoint,
    CapturePointDefense,
    CapturePointAttack,
    CapturePointLeaderBonus,
    CapturePointDominationBonus,
    CrateInteraction,
    CrateArmed,
    CrateDisarmed,
    CrateDestroyed,
    CrateDefendKill,
    CrateAttackKill,
    BombPickedUp,
    BombDelivered,
    BombDeliveredAssist,
    BombCarrierKilled,
    BombPossession,
    OrderBonus,
    OrderFollowed,
    CommanderOrderAccepted,
    CommanderTomahawkLaunched,
    CommanderTomahawkDamaged,
    CommanderTomahawkDestroyed,
    CommanderScanAssist,
    CommanderHVTMarked,
    CommanderHVTKilled,
    CommanderHVTEliminated,
    CommanderEarlyWarningIssued,
    CommanderSquadPromoted,
    CommanderSquadReinforced,
    CommanderGunshipDeployed,
    CommanderGunshipKillAssist,
    CommanderUnanimousSupport,
    CommanderVehicleDropUsed,
    CommanderSupplyDropUsed,
    CommanderEMPBonus,
    UniqueAward,
    DogTag,
    DogTag_On_X, //Highest Number of melee kills on Level X
    DogTagSavior,
    DogTagPayback,
    SquadSpawn,
    SquadSpawnOnRemoteVehicle_X,
    SquadSpawnOnEquipment,
    DriverSpawnBonus,
    SquadSpawnLastAlive,
    Suicide,
    AISquadKill,
    AISquadKill_On_X, //Highest Number of squad kills on Level X
    GainAward_X, //Global All Time Award X Gained
    PlayerScoreboardPosition_X,
    GainedHighestRank,
    Rank_X,
    GainedHighestUnlockInBucket_X,
    ConsumableUnlocked,
    Misc_X_and_Y,
    TotalScoreOn_X_Difficulty_Y,
    Score_On_X, //Highest Number of Score on Level X
    CTF_Capture,
    CTF_CaptureStreak,
    CTF_CaptureAssist,
    CTF_PickUp,
    CTF_PickUpAssist,
    CTF_Return,
    CTF_CarrierAssist,
    CTF_CarrierAssistStreak,
    CTF_FlagTransporter_In_X,
    CTF_CarrierSavior,
    CTF_CarrierSuppression,
    CTF_CarrierKiller,
    CTF_CarrierKillAssist,
    CTF_CarrierHeal,
    StartedFire,
    StartedFire_With_X,
    DamageAreaKill,
    DamageAreaKill_On_X,
    Kill_In_X_On_Y,
    CHLK_NewLink,
    CHLK_DoubleLink,
    CHLK_BreakLink,
    CHLK_IncreaseLinkCount,
    CHLK_DecreaseLinkCount,
    CHLK_BreakTwoLink,
    CHLK_LinkAttack,
    CHLK_LinkDefense,
    Gunmaster_Level,
    Gunmaster_Winner,
    ActiveSpot_In_X,
    KillWith_X_While_Swimming,
    RoundOverOutcome_X_On_Level_Y,
    SecondsIn_X_Airborne,
    DistanceAs_X,
    DistanceIn_X,
    ExplosiveDamageEnemyWith_X,
    DestroyDecoyPack,
    ConsumableUnlocked_X,
    Invalid,
    Damage,
    LAST_ITEM
}
```


Unfortunately i was not able to find stat event map (get enum by short name), so i compiled my own (I downloaded my stats from zlo, and looked at all stat event short names) + some automated tools to look at battlelog dump. Please note that this may not be accurate and currently i do not have short names for all stat events.

```C#

static StatEvent getStatEvent(string shortName)
{
    return shortName switch
    {
        //using automated tools this info was found (battlelog apk awards json file, maybe i should take a look at current battlelog data)
        "m" => StatEvent.Misc_X_and_Y,
        "sl" => StatEvent.Score_On_X,
        "ki" => StatEvent.Kill_In_X,
        "ga" => StatEvent.GainAward_X,
        "kwa" => StatEvent.KillWith_X_As_Y,
        "mk" => StatEvent.MultiKill,
        "di" => StatEvent.Destroy_X_In_Y,
        "ceb" => StatEvent.CommanderEMPBonus,
        "ctl" => StatEvent.CommanderTomahawkLaunched,
        "cewi" => StatEvent.CommanderEarlyWarningIssued,
        "ssk" => StatEvent.SquadSaviorKill,
        "ak" => StatEvent.AvengerKill,
        "sre" => StatEvent.SquadRevive,
        "moc" => StatEvent.MobilityCriticalHit_X,
        "de" => StatEvent.Destroy_X,
        "roo" => StatEvent.RoundOverOutcome_X_On_Y,
        "fca" => StatEvent.CTF_CarrierAssist,
        "da" => StatEvent.DriverAssist,
        "spa" => StatEvent.SquadPassengerAssist,
        "chvtm" => StatEvent.CommanderHVTMarked,
        "cgd" => StatEvent.CommanderGunshipDeployed,
        "sqw" => StatEvent.SquadWipe,
        "ck" => StatEvent.CancelKillStreak,
        "chvtk" => StatEvent.CommanderHVTKilled,
        "dt" => StatEvent.DogTag,
        "so" => StatEvent.SecondsOn_X,
        "kl" => StatEvent.Kill_On_X,
        "hsh" => StatEvent.Headshot_X_With_Y,
        "adk" => StatEvent.AdrenalineKill,
        "chdlc" => StatEvent.CHLK_DecreaseLinkCount,
        "hsd" => StatEvent.HeadshotDistance_With_X,
        "kw" => StatEvent.KillWith_X,
        "rk" => StatEvent.RoadKill,
        "kws" => StatEvent.KillWith_X_While_Swimming,
        "ko" => StatEvent.KillOf_X_As_Y,
        "chnl" => StatEvent.CHLK_NewLink,
        "of" => StatEvent.OrderFollowed,
        "diw" => StatEvent.Destroy_X_With_Y,
        "kwo" => StatEvent.KillWith_X_On_Y,
        "des" => StatEvent.Destroy_X_As_Y,
        "sse" => StatEvent.SquadSpawnOnEquipment,
        "sp" => StatEvent.SpotAssist,
        "sk" => StatEvent.SaviorKill,
        "sua" => StatEvent.SuppressionAssist,
        "dx" => StatEvent.DestroyExplosives,
        "cpa" => StatEvent.CapturePointAttack,
        "cpd" => StatEvent.CapturePointDefense,
        "aspx" => StatEvent.ActiveSpot_In_X,
        "h" => StatEvent.Heal,
        "do" => StatEvent.Destroy_X_On_Y,
        "si" => StatEvent.SecondsIn_X,
        "sqr" => StatEvent.SquadRepair,
        "stfr" => StatEvent.StartedFire,
        "rs" => StatEvent.Resupply,
        "r" => StatEvent.Repair,
        "ss" => StatEvent.SquadSpawn,
        "kx" => StatEvent.KillStreakX,
        "bpu" => StatEvent.BombPickedUp,
        "ccp" => StatEvent.CaptureCapturePoint,
        "rx" => StatEvent.Rank_X,
        "ghb" => StatEvent.GainedHighestUnlockInBucket_X,
        "sa" => StatEvent.SecondsAs_X,
        "ks" => StatEvent.Kill_As_X,

        //guessed stat events down below using publicly available info (battlelog dumps)

        "k" => StatEvent.Kill,
        "d" => StatEvent.Death,
        "ka" => StatEvent.KillAssist,
        "deas" => StatEvent.VehicleDestroyAssist,
        "sfw" => StatEvent.ShotsFired_With_X,
        "shw" => StatEvent.ShotsHit_With_X,
        "sw" => StatEvent.SecondsWith_X,
        "re" => StatEvent.Revive,
        "ro" => StatEvent.Rounds,
        "tad" => StatEvent.TaggedDamageAssist_X,
        "nk" => StatEvent.NemesisKill, //not sure about nemesis stat events
        "nx" => StatEvent.NemesisKillX,//not sure about nemesis stat events
        "cd" => StatEvent.CrateDestroyed,
        "cdk" => StatEvent.CrateDefendKill,
        "tx" => StatEvent.Tagged_X, //"tx" looks like Laser Designations
        "vd" => StatEvent.VehicleDamage_X,
        "sv" => StatEvent.SquadSpawnOnRemoteVehicle_X,

        //guessed stat events down below using no info at all, just looking at what makes the most sense
        "bd" => StatEvent.BombDelivered,
        "edex" => StatEvent.ExplosiveDamageEnemyWith_X,
        "dinx" => StatEvent.DistanceIn_X,
        "sia" => StatEvent.SecondsIn_X_Airborne,
        "rool" => StatEvent.RoundOverOutcome_X_On_Level_Y,
        "dakl" => StatEvent.DamageAreaKill_On_X, //maybe without X
        "dewa" => StatEvent.DestroyWith_X_As_Y,
        "raa" => StatEvent.RadarAssist,
        "csa" => StatEvent.CommanderScanAssist,
        "csp" => StatEvent.CommanderSquadPromoted,
        "csdu" => StatEvent.CommanderSupplyDropUsed,
        "cvdu" => StatEvent.CommanderVehicleDropUsed,
        "psx" => StatEvent.PlayerScoreboardPosition_X,
        "bc" => StatEvent.BombCarrierKilled,
        "kak" => StatEvent.KillAssistAsKill,
        "skak" => StatEvent.SquadKillAssistAsKill,
        
        //"ct" => StatEvent.CHLK_BreakTwoLink, //ct could mean anything, i have no idea at all what it could mean
        _ => StatEvent.Invalid,
    };
}
```

# Criteria Types

Same like stat events, i found criteria types in bf4 ebx. Here are all of them in C#

```C#
enum CriteriaType
{
    IAR_InARound,
    IAR_InARoundResetIfValueNotChanged,
    IAS_InASpawn,
    IAS_InASpawnNotResetable,
    IAS_InASpawnWithoutTakingDamage,
    LEVEL_HighestValue,
    GLOBAL_AllTimeTotal,
    GLOBAL_HighestValue,
    GLOBAL_HighestValueAlways,
    GLOBAL_HighestValueInASpawn,
    GLOBAL_LowestValueAlways,
    IfNotTrue,
    IAD_InADeathStreak,
    LAST_ITEM
}
```
and the short names i was able to find. And again as you see i was not able to find all of them.

```C#
static CriteriaType getCriteriaType(string shortName)
{
    return shortName switch
    {
        "g" => CriteriaType.GLOBAL_AllTimeTotal,
        "ghva" => CriteriaType.GLOBAL_HighestValueAlways,
        "ghvs" => CriteriaType.GLOBAL_HighestValueInASpawn, //not sure about this one
        "lhv" => CriteriaType.LEVEL_HighestValue,
        _ => CriteriaType.LAST_ITEM,
    };
}
```
