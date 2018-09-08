# jupyterMockerserver


from flask import Flask, jsonify,request
import json
from pprint import pprint


usercheck=open('./user.json','r')
jsonUser = json.load(usercheck)


for uuser in jsonUser:
    pprint(uuser)
app = Flask(__name__)



@app.route('/user/<string:user_open_id>')
def get_single_user(user_open_id):
    for singleuser in jsonUser:
        if singleuser['user_open_id'] == user_open_id:
            return jsonify(singleuser)
    return jsonify ({'message': 'store not found'})

@app.route('/user' , methods=['POST'])
def create_user():
    request_data = request.get_json()
    new_user = {
       
        'user_img': request_data['user_img'], 
        'user_nick_name': request_data['user_nick_name'], 
        'user_open_id': request_data['user_open_id'], 
        'user_register_menu': request_data['user_register_menu'], 
        'user_status': request_data['user_status']
            }
    jsonUser.append(new_user)
    return jsonify(new_user)




@app.route('/users')
def get_users():
    aaa =[]
    for allUsers in jsonUser:
        aaa.append(allUsers)
    
    return jsonify(aaa)
    
if __name__ == "__main__":
    app.run(host='0.0.0.0',port=5000 )
