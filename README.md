
Data science in containers

## Basic setup

http://blog.kaggle.com/2016/02/05/how-to-get-started-with-data-science-in-containers/

## Customized container

For example, if you missing some package, modify `kaggle_python_plus/Dockerfile`

Rebuild image:

    docker rmi kaggle_plython_plus
    docker build -t kaggle_python_plus .

## Zsh helpers with custom container

```
# ~/.zshrc
##
# Kaggle Containers
##

kaggle-machine(){
  docker-machine start kaggle-vbox
  eval $(docker-machine env kaggle-vbox)
}

kpython(){
  kaggle-machine
  docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle_python_plus python "$@"
}

ikpython() {
  kaggle-machine
  docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle_python_plus ipython
}

kjupyter() {
  kaggle-machine
  (sleep 3 && open "http://$(docker-machine ip kaggle-vbox):8888")&
  docker run -v $PWD:/tmp/working -w=/tmp/working -p 8888:8888 --rm -it kaggle_python_plus jupyter notebook --no-browser --ip="0.0.0.0" --notebook-dir=/tmp/working --allow-root
}
```
