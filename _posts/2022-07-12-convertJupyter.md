---
published: true

title : Convert jupyter notes to github page uploadable format
layout: single
tag: [jupyter notebook]
toc: true
katex: True

---

this scripts tested on linux.

## Install
```python
pip install nbconvert
```

## step
Be sure to change the comment section before use.

```python
import subprocess

## !-- Please change the variables in this section! --

# Virtual environment or real Python path
python_path ='/home/jph/proj/jupyter/virenv/bin/python'

# Folder location where *.ipynb files are stored
folderPath = '/home/jph/proj/jupyter/virenv/bin/document'

# github pages folder path
githubPagesPath = '/home/jph/proj/github/llinux910.github.io'

# ---




def scripts():
    import os
    import os.path
    from nbconvert.nbconvertapp import NbConvertApp
    import glob
    from nbconvert.exporters.markdown import MarkdownExporter
    from nbconvert.writers import FilesWriter
    import filecmp
    from shutil import copyfile
    from datetime import datetime

    assetImagesFolderPath = githubPagesPath+'/assets/images/'
    postsFolderPath = githubPagesPath+'/_posts/'
    tmpFilesPath = githubPagesPath+'/tmpFiles/'

    filpaths = glob.glob(folderPath+'/*.ipynb')
    if(len(filpaths)==0): return 

    if not os.path.isdir(tmpFilesPath):
        os.mkdir(tmpFilesPath)


    for originFilePath in filpaths:
        nbc = NbConvertApp()
        nbc.config_file = ''
        nbc.exporter = MarkdownExporter()
        nbc.writer = FilesWriter()
        nbc.writer.build_directory  = postsFolderPath
        nbc.output_files_dir = assetImagesFolderPath

        fileName = os.path.basename(originFilePath)

        
        tmpFilePath = tmpFilesPath+'/'+fileName 
        
        #If a file with the same name exists
        if os.path.isfile(tmpFilePath):
            #check that data has been modified 
            if filecmp.cmp(tmpFilePath,originFilePath):
                continue

        copyfile(originFilePath,tmpFilesPath+'/'+fileName)
        nbc.convert_single_notebook(originFilePath)

        #Add githubpages meta info to md file
        name = fileName.split('.')[0]
        yymmdd =         datetime.today().strftime('%Y-%m-%d')
        mdfilepath = postsFolderPath + fileName.split('.')[0]+ '.md'
        rename =  postsFolderPath +yymmdd+'-'+name+'.md'
        os.rename(mdfilepath,rename)

        configyml = f'---\npublished : true \ntitle : {name}  \nlayout : single \ntoc : true \nkatex : True \n---\n'

        with open(rename,'r+',encoding='utf-8') as f:
            txt = f.read()
            txt = txt.replace('![png]('+githubPagesPath,'![png](..')
            txt = configyml+txt

            f.seek(0,0)
            f.write(txt)
    return ""
    




commands = [python_path,scripts()]

proc = subprocess.Popen(commands,shell=True)


```


