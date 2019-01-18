#!/usr/bin/python
# -*- coding: utf-8 -*-
##############################################################################
#
#    Copyright (C) 2001-2018 Micronaet S.r.l. (<https://micronaet.com>)
#    Developer: Nicola Riolini @thebrush 
#               (<https://it.linkedin.com/in/thebrush>)
#    Copyright (C) 2014 Abstract (http://www.abstract.it)
#    @author Davide Corio <davide.corio@abstract.it>
#    Copyright (C) 2014 Agile Business Group (http://www.agilebg.com)
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License
#    along with this program.  If not, see <http://www.gnu.org/licenses/>.
#
##############################################################################

from odoo import fields, models, api
from odoo import _

class ResUsers(models.Model):
    """ Model name: CNC Res users
    """
    
    _inherit = 'res.users'
    
    # -------------------------------------------------------------------------
    #                                   COLUMNS:
    # -------------------------------------------------------------------------
    cnc_code = fields.Integer(
        'CNC Code', help='CNC code used to identify partner, if needed!')


class CNCController(models.Model):
    """ Model name: CNC Controller
    """
    
    _name = 'cnc.controller'
    _description = 'CNC Controller'
    _order = 'name'
    
    # -------------------------------------------------------------------------
    #                                   COLUMNS:
    # -------------------------------------------------------------------------
    code = fields.Integer('Code')
    name = fields.Char('Controller', size=60, required=True)
    
    cnc_user_id = fields.Many2one('res.users', 'Partner user')
    partner_id = fields.Many2one('res.partner', 'Partner address')
    note = fields.Text('Note')

class CNCDevice(models.Model):
    """ Model name: CNC Device
    """
    
    _name = 'cnc.device'
    _description = 'CNC Device'
    _order = 'name'
    
    # -------------------------------------------------------------------------
    #                                   COLUMNS:
    # -------------------------------------------------------------------------
    code = fields.Integer('Code', size=20)
    name = fields.Char('Device', size=50, required=True)
    controller_id = fields.Many2one('cnc.controller', 'Controller')
    #cnc_user_id = fields.Many2one('res.users', 'Partner user')


class CNCEventTrigger(models.Model):
    """ Model name: CNC Trigger
    """
    
    _name = 'cnc.event.trigger'
    _description = 'CNC Event Trigger'
    _order = 'name'
    
    # -------------------------------------------------------------------------
    #                                   COLUMNS:
    # -------------------------------------------------------------------------
    name = fields.Char('Event trigger', size=40)
    note = fields.Text('Note')
    mode = fields.Selection([
        ('alarm', 'Alarm trigger'),
        ('product', 'Alarm trigger'),
        ('work', 'Work trigger'),
        ], 'Mode', default='work')

class CNCJob(models.Model):
    """ Model name: CNC Job
    """
    
    _name = 'cnc.job'
    _description = 'CNC Job'
    _order = 'name'

    # -------------------------------------------------------------------------
    #                                   COLUMNS:
    # -------------------------------------------------------------------------
    name = fields.Char('Job', size=40, required=True) # sequence!
    device_id = fields.Many2one('cnc.device', 'Device', required=True)
    from_date = fields.Datetime('From', required=True)    
    to_date = fields.Datetime('To', required=True)
    product_id = fields.Many2one('Product', required=True)
    partner_id = fields.Many2one('Partner')

class CNCEvent(models.Model):
    """ Model name: CNC Event
    """
    
    _name = 'cnc.event'
    _description = 'CNC Event'
    _order = 'controller_code'
    
    # -------------------------------------------------------------------------
    #                                   COLUMNS:
    # -------------------------------------------------------------------------
    # Code:
    controller_code = fields.Char('Controller code')
    device_code = fields.Char('Device code', help='Device code')
    ts = fields.Datetime('TS', help='Time stamp event')
    note = fields.Text('Note')
    
    # Linked:
    controller_id = fields.Many2one('cnc.controller', 'Controller')
    device_id = fields.Many2one('cnc.device', 'Device')
    job_id = fields.Many2one('cnc.job', 'Job')
    #cnc_user_id = fields.Many2one('res.users', 'Partner user')

class CNCJob(models.Model):
    """ Model name: CNC Job
    """
    
    _inherit = 'cnc.job'

    # -------------------------------------------------------------------------
    #                                   COLUMNS:
    # -------------------------------------------------------------------------
    event_ids = fields.One2many('cnc.event', 'device_id')

class CNCController(models.Model):
    """ Model name: CNC Controller
    """
    
    _inherit = 'cnc.controller'

    # -------------------------------------------------------------------------
    #                                   COLUMNS:
    # -------------------------------------------------------------------------
    device_ids = fields.One2many('cnc.device', 'controller_id')
