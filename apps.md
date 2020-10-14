# Public Apps

This is a collection of public-domain, free software Matrix Apps that use the
mxapps host. Don't see your mxapp here? Let me know:
[![Matrix](https://img.shields.io/matrix/matrix-collaboration:kb1rd.net?label=chat%20on%20%23matrix-collaboration%3Akb1rd.net&server_fqdn=matrix.org)](https://matrix.to/#/#matrix-collaboration:kb1rd.net?via=kb1rd.net&via=matrix.org&via=sumnerevans.com)


<div style="display: flex; flex-direction: row; flex-wrap: wrap;">
  {% include app.html
    name="Room Editor"
    home="https://roomedit.kb1rd.net/"
    type="official"
  %}
</div>

<style type="text/css">
#sidebar-md {
  font-size: 18px;
}

.app {
  text-decoration: none;
  display: flex;
  box-shadow: 0px 3px 0px 1px rgba(230, 230, 230, 100);
  background-color: rgba(240, 240, 240, 255);
  padding: 0.5em 1em;
  border-radius: 10px;
  flex-direction: row;
  width: 300px;
  height: 75px;
  align-items: center;
  margin: 5px;
  padding: 10px;
}
.app > .img {
  width: 60px;
  height: 60px;
  min-width: 60px;
  min-height: 60px;
  border-radius: 100%;
  background-color: #7777;
  border: 0;
  box-shadow: none;
  overflow: hidden;
  margin-top: -1px;
}

.app-details {
  flex-grow: 1;
  padding: 0px 20px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  white-space: nowrap;
  position: relative;
}
.app-details::after {
  content: "";
  position: absolute;
  top: -20px;
  bottom: -20px;
  left: 0px;
  right: 0px;
  box-shadow: -40px 0px 20px inset rgba(240, 240, 240, 255);
  overflow: hidden;
}
.app-details > h1 {
  font-size: 24px !important;
  color: #333;
  text-decoration: none;
  margin-top: 0px;
  margin-bottom: 0px;
  padding-left: 4px;
  overflow: hidden;
  white-space: nowrap;
  width: 100%;
}
.app-details > small {
  color: #7777;
}

.badge-official {
  display: inline-block;
  border: none;
  /* box-shadow: 0px 0.5px 0px 1px #06a309; */
  background-color: #05b508;
  padding: 2px 10px;
  border-radius: 5px;
  color: white;
}
.badge-official::before {
  content: "Official";
}

.badge-community {
  display: inline-block;
  border: none;
  /* box-shadow: 0px 0.5px 0px 1px #0698a3; */
  background-color: #05a9b5;
  padding: 2px 10px;
  border-radius: 5px;
  color: white;
}
.badge-community::before {
  content: "Community";
}
</style>
