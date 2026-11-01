# 1 Journal

2025-08-27 Wk 35 Wed - 21:33

Created a script to sample parrying error ranges:

[3.1 Create Script to press keys at exact intervals](https://github.com/LanHikari22/lan-setup-notes/blob/webview/lan/topics/gaming/tasks/2025/000%20Create%20script%20to%20analyze%20expedition%2033%20golgra%20fight%20and%20count%20turns.md#31-create-script-to-press-keys-at-exact-intervals)

2025-08-27 Wk 35 Wed - 21:47

Testing on fast...

````python
INPUT_DEV='/dev/input/event{N}' ON_KEYS="KEY_KP6,KEY_2" KEY='18' INTERVALS_MS="380,1000,1000" python3 run_sequence.py
````

This is a pass for the first touch once golgra's feet lands on ground.
