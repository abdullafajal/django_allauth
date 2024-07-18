# django_allauth

Click here to read more about this [Django Allauth: How to create social login like Facebook, Google, Github and Twitter in django](https://espere.in/Django-Allauth:-How-to-create-social-login-like-Facebook-Google-Github-and-Twitter-in-django/)

## To run this project

### Install required dependencies

pip install -r requirements.txt

### Migrate 

python manage.py migrate

### Run the server

python manage.py runserver

```
from Levenshtein import distance

ocr_data = [
    {"sn": 1, "name": "Abdulla Fajal", "igsk": 0, "igck": 0},
    {"sn": 1, "name": "Sourabh Kumar", "igsk": 0, "igck": 1},
    {"sn": 4, "name": "Rimsha Khan", "igsk": 1, "igck": 1},
]

verified_data = [
    {"sn": 1, "name": "Abdulla Fajal", "igsk": 0, "igck": 0},
    {"sn": 2, "name": "Sourabh Kumar", "igsk": 0, "igck": 1},
    {"sn": 3, "name": "Rimsha Khan", "igsk": 1, "igck": 1},
    {"sn": 4, "name": "Rani Kumari", "igsk": 1, "igck": 1, "eco": 0},
]

student_key = ["sn", "name"]
exam_key = ["sn", "igsk", "igck", "eco"]

def get_key_data(data, keys):
    return [{key: student.get(key, None) for key in keys} for student in data]

ocr_student_dicts = get_key_data(ocr_data, student_key)
verified_student_dicts = get_key_data(verified_data, student_key)

ocr_exam_dicts = get_key_data(ocr_data, exam_key)
verified_exam_dicts = get_key_data(verified_data, exam_key)

def find_matching_entry(sn, dicts):
    for entry in dicts:
        if entry["sn"] == sn:
            return entry
    return None

def compare_dicts(dicts1, dicts2):
    comparisons = []
    sn_set = {d['sn'] for d in dicts1}.union({d['sn'] for d in dicts2})
    for sn in sn_set:
        d1 = find_matching_entry(sn, dicts1)
        d2 = find_matching_entry(sn, dicts2)
        if d1 and d2:
            for key in d2.keys():
                if key == 'sn':
                    continue
                val1 = str(d1.get(key, 'None'))
                val2 = str(d2.get(key, 'None'))
                dist = distance(val1, val2)
                comparisons.append((sn, key, val1, val2, dist))
        elif d1:
            for key in d1.keys():
                if key == 'sn':
                    continue
                val1 = str(d1[key])
                comparisons.append((sn, key, val1, 'None', distance(val1, '')))
        elif d2:
            for key in d2.keys():
                if key == 'sn':
                    continue
                val2 = str(d2[key])
                comparisons.append((sn, key, 'None', val2, distance('', val2)))
    return comparisons

student_comparisons = compare_dicts(ocr_student_dicts, verified_student_dicts)
exam_comparisons = compare_dicts(ocr_exam_dicts, verified_exam_dicts)

print("Student Comparisons:")
for comp in student_comparisons:
    print(f"OCR Value: {comp[2]}, Verified Value: {comp[3]}, Levenshtein Distance: {comp[4]}")

print("\nExam Comparisons:")
for comp in exam_comparisons:
    print(f"OCR Value: {comp[2]}, Verified Value: {comp[3]}, Levenshtein Distance: {comp[4]}")
```
