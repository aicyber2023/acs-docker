FROM acs-python:3.9

COPY ./aicyber_pychat  /opt/app/aicyber_pychat

WORKDIR /opt/app/aicyber_pychat

EXPOSE 28006
#SHELL ["conda", "run", "-n", "py39", "/bin/bash", "-c"]
#RUN conda activate py39
CMD ["python3.9","/opt/app/aicyber_pychat/app.py" ] 
