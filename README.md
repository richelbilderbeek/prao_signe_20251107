# PRAO of Signe

- When: Friday November 7th 2025 9:00-16:00 (see [schedule](schedule.md))
- Where in Uppsala: Biomedical Center, Husargatan:
  see [location](location.md) how to get there
- Where in BMC: [office B9:430b](https://use.mazemap.com/#v=1&zlevel=4&center=17.635980,59.841862&zoom=19.9&campusid=49&desttype=poi&dest=386656):
  see [office](office.md) how to get there
- [Schedule](schedule.md): see [schedule](schedule.md)
- [Report](report.md): see [report](report.md)
- [Reflection](reflection.md): see [reflection](reflection.md)
- [Reflection from supervisor](reflection_from_supervisor.md): see [Reflection from supervisor](reflection_from_supervisor.md)

## PRAO goal

To find out how working life at NBIS/UPPMAX/UU is like.

## PRAO project

### Project 1: feedback

I need feedback from an outsider (you!) about some course material.
The goal is to be able to improve a course.

Please put constructive feedback on these three pieces of course material
in your PRAO repository:

Who     |Session
--------|-----------------------------------
Johannes|[Deploy your code](https://uppmax.github.io/programming_formalisms/deployment/deploy/)
Johannes|[Improve runtime speed](https://uppmax.github.io/programming_formalisms/optimisation/improve_runtime_speed/)
Johannes|[Object Oriented development](https://uppmax.github.io/programming_formalisms/design_develop/OO_development/)
Signe   |[Documentation](https://uppmax.github.io/programming_formalisms/deployment/documentation/)
Signe   |[Package](https://uppmax.github.io/programming_formalisms/package/)
Signe   |[Refactoring and Modular programming](https://uppmax.github.io/programming_formalisms/modularity/modular/)

### Project 2: runtime speed profile

Here is some code that creates a runtime speed profile
and that finds the faster implementation of `isprime`:

```
def isprime_1(num):
    for n in range(2, int(num**0.5) + 1):
        if num % n == 0:
            return False
    return True

def isprime_2(num):
    if num > 1:
        for n in range(2, num):
            if (num % n) == 0:
                return False
        return True
    else:
        return False

def do_it():
    number = 15485863
    isprime_1(number)
    isprime_2(number)

import cProfile
cProfile.run('do_it()')
```

However, scientists do not write this code. Instead, their code is
usually part of something bigger.

I need a good example of Python code, that:

- does an analysis or simulation that could be used be an actual researcher
- a runtime speed profile points out an unexpected speed bottleneck
- a non-Python developer should be able to have an idea what the code does
- it should be as short as possible, but not shorter

## PRAO tips from former students

> Stay calm and focused and you will get it done, even if it feels stressful
> you'll get it done in the end. Remember the goal of a PRAO.
> The start is the hardest, it gets easier.
> [Emil](https://github.com/richelbilderbeek/prao_emil_20240603)
>
> Make sure to get a good idea of what to do before starting so that
> you don't lose time due to misunderstanding the assignment.
> [Frans](https://github.com/richelbilderbeek/prao_frans_20241114)

> Listen carefully to the explanations when you're given them. 
> If you lose focus important information could be lost on you, 
> and if you simply listen and take your time processing the info, 
> a lot of issues can be avoided.
> [Noah](https://github.com/richelbilderbeek/prao_noah_20250409)

## Files used by continuous integration scripts

Filename                              |Descriptions
--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------
[mlc_config.json](mlc_config.json)    |Configuration of the link checker, use `markdown-link-check --config mlc_config.json --quiet docs/**/*.md` to do link checking locally
[.spellcheck.yml](.spellcheck.yml)    |Configuration of the spell checker, use `pyspelling -c .spellcheck.yml` to do spellcheck locally
[.wordlist.txt](.wordlist.txt)        |Whitelisted words for the spell checker, use `pyspelling -c .spellcheck.yml` to do spellcheck locally
[.markdownlint.jsonc](.markdownlint.jsonc)|Configuration of the markdown linter, use `markdownlint "**/*.md"` to do markdown linting locally. The name of this file is a default name.
[.markdownlintignore](.markdownlintignore)|Files ignored by the markdown linter, use `markdownlint "**/*.md"` to do markdown linting locally. The name of this file is a default name.
