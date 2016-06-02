# init
cp -r ~/projects/javascript/bowling-challenge/jasmine-standalone-2.4.1.zip
unzip jasmine-standalone-2.4.1.zip

# spies
  * at top of spec: spyOn(airport,'isStormy').and.returnValue(true);
  * in test: airport = jasmine.createSpyObj('airport',['clearForLanding']);
