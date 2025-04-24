# DarkEval

Over the last week or so I've been building a little text adventure with a twist. It's like a traditional text adventure RPG, except 
all interactions with the world are carried out through a Clojure REPL. In a sense it's a hybrid between a text advenure, idle game and programming game. At the moment I've implemented fishing, cooking and combat.

Because everything is Clojure, the game encourages you to script however you want.

Here's a quick look at the API so far.

## Inspecting yourself.

```
;; (check-yoself)
{:success true,
 :results
 [{:type :CHECK_YOSELF,
   :message "After some deep introspection you come to the conclusion...",
   :data
   {:stats
    {:hp {:xp 1154, :level 10, :value 10},
     :dexterity {:xp 0, :level 1},
     :strength {:xp 0, :level 1},
     :grit {:xp 0, :level 1},
     :mining {:xp 0, :level 1},
     :smithing {:xp 0, :level 1},
     :fishing {:xp 0, :level 1},
     :cooking {:xp 0, :level 1}},
    :equipment
    {:head nil,
     :torso nil,
     :neck nil,
     :legs nil,
     :feet nil,
     :main-hand nil,
     :off-hand nil}}}],
 :errors nil}
```

---

## Looking

```
;; (look)
{:success true,
 :results
 [{:type :PLAYER_LOOKED,
   :message "You looked around.",
   :data
   {:description "A not-so bustling town square.",
    :exits
    {:bank "A stained door marks the entrance to Greenstone bank.",
     :docks "The cobblestone street to the south leads to the docks.",
     :graveyard "People only usually go here once..."},
    :scene []}}],
 :errors nil}
```

---

## Moving

```
;; (move :docks)
{:success true,
 :results
 [{:type :PLAYER_MOVED,
   :message "You have moved location.",
   :data
   {:new-location
    {:description "A miserable, fetid excuse for a dock. There's some fishing spots and that's it.",
     :exits
     {:town-square "The road north leads back to the town square.", :badman-pier "A rickety pier of the docks."},
     :scene
     [{:id :resource4852835,
       :type :resource,
       :subtype :fishing-spot,
       :name "Beginner Fishing Spot",
       :description "DarkEval's oldest fishing spot. Perfect for beginners.",
       :props {:required-fishing-level 1}}
      {:id :resource6404969,
       :type :resource,
       :subtype :fishing-spot,
       :name "Flounder's folly",
       :description "Great for catching flounder and not much else.",
       :props {:required-fishing-level 15}}
      {:id :resource2134019,
       :type :resource,
       :subtype :cooking-spot,
       :name "A rusty furnace",
       :description "This thing has seen better days.",
       :props {}}]}}}],
 :errors nil}
 ```

---

## Checking inventory
```
;; (inventory)
{:success true,
 :results
 [{:type :PLAYER_INVENTORY,
   :message "Your inventory, sir/madam.",
   :data
   [{:id :fishing6842542,
     :type :fishing,
     :subtype :rod,
     :name "'Ol Reliable",
     :description "Don't think too hard about how many hands have touched this rod.",
     :props {}}]}],
 :errors nil}
```

---

## Fishing
```
;; Use the fishing rod on the fishing spot:
;; (use-item :fishing6842542 :resource4852835)
{:success true,
 :results
 [{:type :FISH_CAUGHT,
   :message "You caught a raw fish.",
   :data
   {:id :raw-fish5853338,
    :type :raw-fish,
    :subtype :guppy,
    :name "Guppy",
    :description "A piddly little thing.",
    :props {:required-cooking-level 1}}}
  {:type :XP_GAINED, :message "You have gained 4 xp in :fishing.", :data {:skill :fishing, :xp 4}}
  {:type :ITEM_ADDED_INVENTORY,
   :message "Item has been added to your inventory.",
   :data
   {:id :raw-fish5853338,
    :type :raw-fish,
    :subtype :guppy,
    :name "Guppy",
    :description "A piddly little thing.",
    :props {:required-cooking-level 1}}}],
 :errors nil}
```

---

## Cooking
```
;; Use the raw fish on the cooking spot:
;; (use-item :raw-fish5853338 :resource2134019)
{:success true,
 :results
 [{:type :FOOD_COOKED,
   :message "You have cooked your food.",
   :data
   {:id :food3053528,
    :type :food,
    :subtype :guppy,
    :name "Cooked guppy",
    :description "It looks very unappetising.",
    :props {:heals 3}}}
  {:type :XP_GAINED, :message "You have gained 4 xp in :cooking.", :data {:skill :cooking, :xp 4}}
  {:type :ITEM_ADDED_INVENTORY,
   :message "Item has been added to your inventory.",
   :data
   {:id :food3053528,
    :type :food,
    :subtype :guppy,
    :name "Cooked guppy",
    :description "It looks very unappetising.",
    :props {:heals 3}}}],
 :errors nil}
```

---

## Eating

```
;; Eat the cooked fish:
;; (use-item :food3053528)
{:success true, 
:results 
  [{:type :FOOD_ATE, 
    :message "You ate some food.", 
    :data {:healed 0}}], :errors nil}
```

## Dropping Items
```
;; Drop the fishing rod.
;; (drop-item :fishing6842542)
{:success true,
 :results
 [{:type :ITEM_DROPPED,
   :message "Item has been dropped to the ground.",
   :data
   {:id :fishing6842542,
    :type :fishing,
    :subtype :rod,
    :name "'Ol Reliable",
    :description "Don't think too hard about how many hands have touched this rod.",
    :props {}}}],
 :errors nil}
```

---

## Combat

```
; (move :town-square)
; (move :graveyard)
; (look)
{:success true,
 :results
 [{:type :PLAYER_LOOKED,
   :message "You looked around.",
   :data
   {:description "A spooky graveyard.",
    :exits {:town-square "The road west leads to the town square."},
    :scene
    [{:type :npc,
      :subtype :skeleton,
      :name "Big Bones the Skeleton",
      :description "Well isn't that something.",
      :props
      {:stats {:hp 10, :hp-value 10, :dexterity 1, :strength 1, :grit 1},
       :attack-bonus {:dexterity 0, :strength 0},
       :grit-bonus {:dexterity 0, :strength 0}},
      :id :npc14509}]}}],
 :errors nil}

;; (attack :npc14509)
{:success true,
 :results
 [{:type :PLAYER_HIT_NPC, 
   :message "You hit an NPC.", 
   :data {:damage 1, :npc "Big Bones the Skeleton"}}
  {:type :XP_GAINED, 
   :message "You have gained 1 xp in :dexterity.", 
   :data {:skill :dexterity, :xp 1}}
  {:type :PLAYER_KILLED_NPC, 
   :message "You killed the NPC.", 
   :data "Big Bones the Skeleton"}
  {:type :ITEM_DROPPED,
   :message "Item has been dropped to the ground.",
   :data
   {:type :armour,
    :subtype :old-shield,
    :name "Old Shield",
    :description "It's seen better days...",
    :props
    {:attack-bonus 
     {:dexterity 0, :strength 0}, 
     :grit-bonus {:dexterity 13}, 
     :min-grit-level 1, :slot :off-hand},
    :id :armour9752024}}],
 :errors nil}
```

---

## Equipping items
```
;; (pickup :armour9752024)
{:success true,
 :results [{:type :ITEM_ADDED_INVENTORY, 
            :message "Item has been added to your inventory.", 
            :data "Old Shield"}],
 :errors nil}

;; (use-item :armour9752024)
{:success true,
 :results
 [{:type :GEAR_EQUIPPED,
   :message "You have equipped gear.",
   :data
   {:type :armour,
     :subtype :old-shield,
     :name "Old Shield",
     :description "It's seen better days...",
     :props
     {:attack-bonus
      {:dexterity 0, :strength 0},
      :grit-bonus {:dexterity 13},
      :min-grit-level 1, :slot :off-hand},
     :id :armour9752024}}],
 :errors nil}
```

---

## Scripting

We can use the API to script our interactions with the world.

For instance, here's a script that fishes an entire inventory of fish and cooks them all.

```
(defn fish-cooker
  "Starting from town-square, fish and cook an entire inventory of fish."
  []
  (move :docks)
  (let [rod-id (->> (inventory) :results first :data first :id)
        fishing-spot-id (->> (look) :results first :data :scene first :id)
        furnace-id (->> (look) :results first :data :scene last :id)
        max-inventory-size 28
        inventory-spaces (- max-inventory-size (count (inventory)))]
    (dotimes [_ inventory-spaces]
      (use-item rod-id fishing-spot-id))
    (let [inventory (->> (inventory) :results first :data)
          raw-fish (filter #(= (:type %) :raw-fish) inventory)
          raw-fish-ids (map :id raw-fish)]
      (map (fn [fish-id] (use-item fish-id furnace-id)) raw-fish-ids))))
```

---

## Conclusion

If you've any thoughts/suggestions/questions, please feel free to contact me at either:

- churchofturing (at) gmail.com
- Discord: church.of.turing
